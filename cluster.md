> 容器化环境准备

* docker安装
1. 移除以前docker
```sh
sudo yum remove -y docker \
                   docker-ce \
                   docker-ce-cli \
                   containerd.io \
                   docker-client \
                   docker-common \
                   docker-latest \
                   docker-engine \
                   docker-logrotate \
                   docker-client-latest \
                   docker-latest-logrotate
```
2. 配置yum源
```sh
sudo yum install -y yum-utils
# 科学上网
sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo
# 不能科学上网
sudo yum-config-manager \
--add-repo \
http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```
3. 安装
```sh
sudo yum install -y docker-ce docker-ce-cli containerd.io --allowerasing
# 也可指定版本安装
```
4. 启动和配置加速
```sh
# 加入开机自启(防火墙打开可能会导致docker.service无法正常运行)
systemctl stop firewalld.service
systemctl disable firewalld.service
systemctl enable docker --now
# 配置加速
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["http://10.166.44.42:5000/"],
  "insecure-registries": ["10.166.44.134:5000"],
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF
```
5. 更改默认存储位置
```sh
vim /usr/lib/systemd/system/docker.service
# --graph your_path
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock --graph /home/docker
```
6. 设置http代理
```sh
# 确保已经正确设置PROXY环境变量
sudo mkdir -p /etc/systemd/system/docker.service.d
printf "[Service]\nEnvironment=\"HTTP_PROXY=$http_proxy\" \"HTTPS_PROXY=$https_proxy\" \"NO_PROXY=$no_proxy\"\n" | sudo tee /etc/systemd/system/docker.service.d/http-proxy.conf
sudo systemctl daemon-reload
sudo systemctl restart docker
```
7. 添加当前用户
```sh
sudo usermod -aG docker $USER
sudo gpasswd -a $USER docker
newgrp docker
```
> k8s集群安装
1. 集群基础环境设置
```sh
# 设置有意义的域名
hostnamectl set-hostname xxx
# 禁用SELinux
sudo setenforce 0
sudo sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
# 关闭swap
swapoff -a  
sed -ri 's/.*swap.*/#&/' /etc/fstab
#允许 iptables 检查桥接流量
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
br_netfilter
EOF

cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
sudo sysctl --system
```
2. 安装kubelet、kubeadm、kubectl
```sh
# 国内
cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=http://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=0
repo_gpgcheck=0
gpgkey=http://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg
   http://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
exclude=kubelet kubeadm kubectl
EOF
# 国外
cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-\$basearch
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kubelet kubeadm kubectl
EOF

sudo yum install -y kubelet-1.21.1 kubeadm-1.21.1 kubectl-1.21.1 --disableexcludes=kubernetes
sudo systemctl enable --now kubelet
```
3. kubeadm初始化集群master
```sh
# 添加域名映射
echo "10.219.138.131  rasp-1637-cluster-endpoint" >> /etc/hosts
# 添加域名的no_proxy
vim /etc/environment
no_proxy=xxx,rasp-1637-cluster-endpoint
source /etc/environment

# 初始化前要保证环境干净（新的环境或reset之前的k8s环境）
kubeadm reset -f
rm -fr /etc/kubernetes && rm -fr /etc/cni/net.d && rm -fr /var/lib/kubelet && rm -fr /var/run/kubernetes && ipvsadm --clear
sh -c "iptables -F && iptables -t nat -F && iptables -t mangle -F && iptables -t raw -F  && iptables -t raw -X && iptables -X && iptables -Z "

# master初始化
kubeadm init --apiserver-advertise-address=10.219.138.131 --control-plane-endpoint=rasp-1637-cluster-endpoint --kubernetes-version v1.21.10 --service-cidr=10.96.0.0/16 --pod-network-cidr=10.244.0.0/16
# 保证所有网络范围不重叠(service-cidr, --pod-network-cidr)

# To start using your cluster, you need to run the following as a regular user
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

# 初始化成功后会有类似下面信息打印，我们需要保存添加node的token命令
You can now join any number of control-plane nodes by copying certificate authorities
and service account keys on each node and then running the following as root:

  # 添加另外的master到集群
kubeadm join rasp-1637-cluster-endpoint:6443 --token o48jat.pu37rajelbpwhkqg \
    --discovery-token-ca-cert-hash sha256:249e8b603a2a9bb557fea69d2f435ff13c9f70c9814d3c11f34410895208d1ef \
    --control-plane

Then you can join any number of worker nodes by running the following on each as root:

# 添加worker到集群
kubeadm join rasp-1637-cluster-endpoint:6443 --token o48jat.pu37rajelbpwhkqg \
        --discovery-token-ca-cert-hash sha256:249e8b603a2a9bb557fea69d2f435ff13c9f70c9814d3c11f34410895208d1ef
```
4. 部署pod网络环境(on master)
```sh
curl https://docs.projectcalico.org/manifests/calico.yaml -O

# update CALICO_IPV4POOL_CIDR in calico.yaml，和init时指定的pod网络保持一致
 - name: CALICO_IPV4POOL_CIDR
   value: "10.244.0.0/16"
 - name: IP_AUTODETECTION_METHOD
   value: "can-reach=10.165.56.108"  #master-IP

kubectl apply -f calico.yaml
```
5. 添加其他节点到集群
* 每个添加到集群的节点都要准备容器环境
```sh
# 添加域名映射
echo "10.219.138.131  rasp-1637-cluster-endpoint" >> /etc/hosts
# 添加域名的no_proxy
vim /etc/environment
no_proxy=xxx,rasp-1637-cluster-endpoint
source /etc/environment

# join节点之前要保证环境干净
kubeadm reset -f
rm -fr /etc/kubernetes
rm -fr /etc/cni/net.d
rm -fr /var/lib/kubelet
rm -fr /var/run/kubernetes
ipvsadm --clear
sh -c "iptables -F && iptables -t nat -F && iptables -t mangle -F && iptables -t raw -F  && iptables -t raw -X && iptables -X && iptables -Z "

# 1. 添加control-plane
kubeadm join rasp-1637-cluster-endpoint:6443 --token o48jat.pu37rajelbpwhkqg \
    --discovery-token-ca-cert-hash sha256:249e8b603a2a9bb557fea69d2f435ff13c9f70c9814d3c11f34410895208d1ef \
    --control-plane
# 2. 添加worker
kubeadm join rasp-1637-cluster-endpoint:6443 --token o48jat.pu37rajelbpwhkqg \
        --discovery-token-ca-cert-hash sha256:249e8b603a2a9bb557fea69d2f435ff13c9f70c9814d3c11f34410895208d1ef
```
