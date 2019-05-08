## Threads and Concurrency

So far, we've only seen programs that run like a single horse, running round a race course. 

However, it often makes sense for a program's *main task* to be split into smaller ones (let's call them *sub-tasks*).  

Imagine an ant-colony, where a large number of worker ants toil together, to complete what the Queen Ant tells them to do. Groups of ants that are part of separately tasks, work with a free hand.

The concept of **concurrency** in programming is very similar to what an ant colony is in nature. 

### Step 01: Concurrent Tasks: Extending ```Thread```

In Java, you can run tasks in parallel using threads.  Let's first write a simple program.

##### Example-1
**SimpleJavaRunner.java_**

```java
package com.amitsa.Multithreading.SimplejavaRunner;

class Runner1 {

    public void startRunning() {
        for(int i=0;i<10;++i)
            System.out.println("Runner1: "+i);
    }
}

class Runner2 {

    public void startRunning() {
        for(int i=0;i<10;++i)
            System.out.println("Runner2: "+i);
    }
}

public class SimpleJavaRunner {

    public static void main(String[] args) {

        Runner1 runner1 = new Runner1();
        Runner2 runner2 = new Runner2();

        runner1.startRunning();
        runner2.startRunning();

    }
}
```
##### Example-1 Explained
Simple sequential java program at first Runner1 startRunning method gets executed and then Runner2.

**_Console Output_**
<br>
/home/amitsah/opt/jdk-11.0.1/bin/java -javaagent:/home/amitsah/opt/idea-IC-183.5912.21/lib/idea_rt.jar=37649:/home/amitsah/opt/idea-IC-183.5912.21/bin -Dfile.encoding=UTF-8 -classpath /home/amitsah/coding_2019/out/production/coding_2019 com.amitsa.Multithreading.SimplejavaRunner.SimpleJavaRunner
Runner1: 0
Runner1: 1
Runner1: 2
Runner1: 3
Runner1: 4
Runner1: 5
Runner1: 6
Runner1: 7
Runner1: 8
Runner1: 9
Runner2: 0
Runner2: 1
Runner2: 2
Runner2: 3
Runner2: 4
Runner2: 5
Runner2: 6
Runner2: 7
Runner2: 8
Runner2: 9

Process finished with exit code 0
<br>
##### Example-2

**_ThreadBasicsRunner.java_**

```java
package com.amitsa.Multithreading.ThreadBasicsRunner
public class ThreadBasicsRunner {
	public static void main(String[] args) {
		//Task1
		for(int i=101; i<=199; i++) {
			System.out.print(i + " ");
		}
		System.out.println("\n Task1 Done");

		//Task2
		for(int i=201; i<=299; i++) {
			System.out.print(i + " ");
		}
	
		System.out.println("\n Task2 Done");
		//Task3
		for(int i=301; i<=399; i++) {
			System.out.print(i + " ");			
		}
		System.out.println("\n Task3 Done");

		System.out.println("Main Done");
	}
}
	
```

**_Console Output_**

_101 102 103 104 105 106 107 108 109 110 111 112 113 114 115 116 117 118 119 120 121 122 123 124 125 126 127 128 129 130 131 132 133 134 135 136 137 138 139 140 141 142 143 144 145 146 147 148 149 150 151 152 153 154 155 156 157 158 159 160 161 162 163 164 165 166 167 168 169 170 171 172 173 174 175 176 177 178 179 180 181 182 183 184 185 186 187 188 189 190 191 192 193 194 195 196 197 198 199_

_Task1 Done_

_201 202 203 204 205 206 207 208 209 210 211 212 213 214 215 216 217 218 219 220 221 222 223 224 225 226 227 228 229 230 231 232 233 234 235 236 237 238 239 240 241 242 243 244 245 246 247 248 249 250 251 252 253 254 255 256 257 258 259 260 261 262 263 264 265 266 267 268 269 270 271 272 273 274 275 276 277 278 279 280 281 282 283 284 285 286 287 288 289 290 291 292 293 294 295 296 297 298 299_

_Task2 Done_

_301 302 303 304 305 306 307 308 309 310 311 312 313 314 315 316 317 318 319 320 321 322 323 324 325 326 327 328 329 330 331 332 333 334 335 336 337 338 339 340 341 342 343 344 345 346 347 348 349 350 351 352 353 354 355 356 357 358 359 360 361 362 363 364 365 366 367 368 369 370 371 372 373 374 375 376 377 378 379 380 381 382 383 384 385 386 387 388 389 390 391 392 393 394 395 396 397 398 399_

_Task3 Done_

_Main Done_

##### Example-2 Explained

As you can see, the execution of all three ```for``` loops (that really are independent tasks) is sequential. This is how all our code so far has been running!

### Thread Creation

There are two ways in which you can create a thread to represent a sub-task, within a program. They are:

* Define your own thread ```class``` to sub-class the **```Thread```** ```class```.
* Define your own thread ```class``` to implement the **```Runnable```** ```interface```.

In this step, we will focus on the first alternative.

##### Snippet-01 : A simple Java thread class

**_ThreadBasicsRunner.java_**

```java

	class Task1 extends Thread {
		public void run() {
			System.out.println("Task1 Started ");
			for(int i=101; i<=199; i++) {
				System.out.print(i + " ");
			}
			System.out.println("\nTask1 Done");
		}	
	}

	public class ThreadBasicsRunner {
		public static void main(String[] args) {
			//Task1
			Task1 task1 = new Task1();
			task1.start();

			//Task2
			for(int i=201; i<=299; i++) {
				System.out.print(i + " ");
			}
			System.out.println("\nTask2 Done");

			//Task3
			for(int i=301; i<=399; i++) {
				System.out.print(i + " ");
			}
			System.out.println("\nTask3 Done");
			System.out.println("\nMain Done");
		}	
	}

```

**_Console Output_**

```
Task1 Started 
201 101 202 102 203 103 204 104 105 205 206 106 207 107 208 108 209 109 210 110 211 111 212 112 213 113 214 114 215 115 216 116 217 218 117 219 118 220 119 221 120 222 121 122 223 123 224 124 225 125 226 126 227 127 228 128 229 129 230 130 231 131 232 132 233 133 234 134 235 135 236 136 237 137 238 138 239 139 240 140 241 141 242 142 243 143 244 144 245 145 246 247 146 248 147 249 148 250 149 251 150 252 151 253 152 254 153 255 154 256 155 257 156 258 157 259 158 260 159 261 160 262 161 263 162 264 163 265 164 266 165 267 166 268 167 269 168 270 169 271 170 272 171 273 172 274 173 275 174 276 175 277 278 279 176 280 177 281 178 179 282 180 181 182 283 183 284 184 285 185 286 186 287 187 288 289 188 290 189 291 190 292 191 293 192 294 193 194 295 195 196 296 197 297 298 299 198 
Task2 Done
301 199 302 
Task1 Done
303 304 305 306 307 308 309 310 311 312 313 314 315 316 317 318 319 320 321 322 323 324 325 326 327 328 329 330 331 332 333 334 335 336 337 338 339 340 341 342 343 344 345 346 347 348 349 350 351 352 353 354 355 356 357 358 359 360 361 362 363 364 365 366 367 368 369 370 371 372 373 374 375 376 377 378 379 380 381 382 383 384 385 386 387 388 389 390 391 392 393 394 395 396 397 398 399 
Task3 Done

Main Done
```

##### Snippet-01 Explained

We defined a ```Task1``` ```class``` to denote our sub-task, with a ```run()``` method definition. However,  when we create such a thread within our ```main()``` method, we don't seem to be invoking ```run()``` in any way! What's happening here? 

A thread can be created and launched, by calling a generic method named ```start()```. method. Calling ```start()``` will invoke the individual thread’s ```run()``` method.

From the console output, we see that the output of *Task1* overlaps with those of tasks labeled *Task2* and *Task3*. *Task1* is running in parallel with main which is running (*Task2*, *Task3*).

#### Summary

In this step, we:

* Discovered how to define a thread by sub-classing ```Thread```
* Demonstrated how to run a Thread

### Step 02: Concurrent Tasks - Implementing Runnable

##### Snippet-01 : Implementing Runnable

In **Step 01**, we told you that there are two ways a thread could represent a sub-task, in a Java program. One was by sub-classing a ```Thread```, and the other way is to implement ```Runnable```. We saw the first way a short while ago, and it's time now to explore the second. The following example will show you how it's done.

**_ThreadBasicsRunner.java_**

```java

	class Task1 extends Thread {
		public void run() {
			System.out.println("Task1 Started ");
			for(int i=101; i<=199; i++) {
				System.out.print(i + " ");
			}
			System.out.println("\nTask1 Done");
		}
	}

	class Task2 implements Runnable {
		@Override
		public void run() {
			System.out.println("Task2 Started ");
			for(int i=201; i<=299; i++) {
				System.out.print(i + " ");		
			}
			System.out.println("\nTask2 Done");
		}
	}

	public class ThreadBasicsRunner {
		public static void main(String[] args) {
			System.out.print("\nTask1 Kicked Off\n");
			Task1 task1 = new Task1();
			task1.start();

			System.out.print("\nTask2 Kicked Off\n");
			Task2 task2 = new Task2();
			Thread task2Thread = new Thread(task2);
			task2Thread.start();

			System.out.print("\nTask3 Kicked Off\n");
			for(int i=301; i<=399; i++) {
				System.out.print(i + " ");
			}
			System.out.println("\nTask3 Done");

			System.out.println("\nMain Done");
		}
	}

```

**_Console Output_**
```

Task1 Kicked Off
Task1 Started 
101 102 103 104 105 106 107 108 109 110 111 
Task2 Kicked Off
112 113 114 115 116 117 118 119 120 121 122 123 124 125 126 127 128 129 130 131 132 133 134 135 136 137 138 139 140 141 142 143 144 145 146 147 148 149 150 151 152 153 154 155 156 157 158 159 160 161 162 163 164 165 166 167 168 169 170 171 172 173 174 175 176 177 178 179 180 181 182 183 184 185 186 187 188 189 190 191 192 193 194 195 196 197 198 199 
Task1 Done

Task3 Kicked Off
Task2 Started 
201 202 203 204 205 206 207 208 209 210 211 212 213 301 214 215 216 217 218 302 219 220 221 222 223 224 225 226 227 228 303 229 230 231 232 233 234 235 236 237 238 239 240 241 242 243 244 245 246 247 248 249 250 251 252 253 254 255 256 257 258 259 260 261 262 263 264 265 266 267 304 268 269 270 271 272 273 274 275 276 277 278 279 280 281 282 283 284 285 286 305 287 288 289 290 291 292 293 294 295 296 297 298 299 
Task2 Done
306 307 308 309 310 311 312 313 314 315 316 317 318 319 320 321 322 323 324 325 326 327 328 329 330 331 332 333 334 335 336 337 338 339 340 341 342 343 344 345 346 347 348 349 350 351 352 353 354 355 356 357 358 359 360 361 362 363 364 365 366 367 368 369 370 371 372 373 374 375 376 377 378 379 380 381 382 383 384 385 386 387 388 389 390 391 392 393 394 395 396 397 398 399 
Task3 Done

Main Done

```


##### Snippet-01 Explained

In this example, we implemented ```Runnable``` by implementing the ```run()``` method from ```Runnable``` interface.

To run Task2 which is implementing `Runnable` interface, we used this code. We are using the `Thread` constructor passing an instance of `Task2`.

```java
Task2 task2 = new Task2();
Thread task2Thread = new Thread(task2);
task2Thread.start();
```

You can see from the output that all three tasks are running in parallel.

#### Summary

In this step, we:

* Explored another way to create threads, by implementing the ```Runnable``` ```interface```
* Learned to run a thread created using ```Runnable``` ```interface```
 
### Step 03: The Thread Life-cycle  

A Java Thread goes through a sequence of **states** during its lifetime. The term **life-cycle** is used to describe this fact, and clearly defines what specific state a thread could be in, at various points of time. 

Let's consider the following example we explored recently, in _**Step 02**_:

```java

	class Task1 extends Thread {
		public void run() {
			for(int i=101; i<=199; i++) {
				System.out.print(i + " ");
			}
		}	
	}

	class Task2 implements Runnable {
		@Override
		public void run() {
			for(int i=201; i<=299; i++) {
				System.out.print(i + " ");
			}
		}
	}

	public class ThreadBasicsRunner {
		public static void main(String[] args) {
			Task1 task1 = new Task1();
			task1.start();
			Task2 task2 = new Task2();
			Thread task2Thread = new Thread(task2);
			task2Thread.start();
			for(int i=301; i<=399; i++) {			
				System.out.print(i + " ");
			}
		}
	}

```

Different states of a thread are:

* **NEW**: A thread is in this state as soon as it's been created, but its ```start()``` method hasn't yet been invoked.
 
	* For *Task1* : After the execution of ```Task1 task1 = new Task1();```
	* For *Task2* : After the execution of ```Task2 task2 = new Task2();  task2Thread = new Thread(task2);```

* **TERMINATED/DEAD**: When all the statements inside a thread's ```run()``` method have been been completed, that thread is said to have terminated.

A thread can be in any one of the remaining three states, after its ```start()``` method has been invoked.
 
* **RUNNING**: If the thread is currently running.

* **RUNNABLE**: If the thread is not currently running, but is ready to do so at any time.

* **BLOCKED/WAITING**: If the thread is not currently running on the processor, but is not ready to execute either. This may be the case if it's waiting for an external resource (such as a user's input) or another thread.

#### Summary

In this step, we:

* Discussed different states of a Thread with an example

### Step 04: Thread Priorities
Java allows you to *request* the thread scheduler, to change the priority of a thread. The priority of any thread always lies in a fixed range - ```MIN_PRIORITY = 1``` to ```MAX_PRIORITY = 10```.  The default priority that's assigned to any thread, is ```NORM_PRIORITY = 5```. 

A request to change this priority is done by invoking the static ```setPriority(int)``` method, available in the ```Thread``` ```class```. This request may or may not be honored in response, so be prepared for that! 

### Step 05: Communicating Threads

Any program where threads are not explicitly created is a single-threaded application. The thread that we refer to here is the *main thread*, executing the program's ```main()``` method.

Sometimes, threads might depend on one another. 

Let consider the example from **Step 02**. We want to add a condition - *Task3* should execute only after *Task1* terminates.

**_ThreadBasicsRunner.java_**

```java

	class Task1 extends Thread {
		public void run() {
			System.out.println("Task1 Started ");
			for(int i=101; i<=199; i++) {
				System.out.print(i + " ");
			}
		}
	}

	class Task2 implements Runnable {
		@Override
		public void run() {
			System.out.println("Task2 Started ");
			for(int i=201; i<=299; i++) {
				System.out.print(i + " ");
			}
			System.out.println("\nTask2 Done");
		}
	}

	public class ThreadBasicsRunner {
		public static void main(String[] args) {
			System.out.print("\nTask1 Kicked Off\n");
			Task1 task1 = new Task1();		
			task1.start();
			System.out.print("\nTask2 Kicked Off\n");
			Task2 task2 = new Task2();
			Thread task2Thread = new Thread(task2);
			task2Thread.start();
		
			task1.join();
			
			System.out.print("\nTask3 Kicked Off\n");
			for(int i=301; i<=399; i++) {
				System.out.print(i + " ");
			}
			System.out.println("\nTask3 Done");
			System.out.println("\nMain Done");
		}
	}

```

##### Snippet-5 Explained

```task1.join()``` waits until task1 completes. So, the code after ```task1.join()``` will executed only on completion of `task1`.

If we want *Task3* to be executed only after both *Task1* and *Task2* are done, the code in ```main()``` needs to look as follows:

##### Snippet-6 : Task3 after Task1 and Task2

**_ThreadBasicsRunner.java_**

```java

	public static void main(String[] args) {
		System.out.print("\nTask1 Kicked Off\n");
		Task1 task1 = new Task1();
		task1.start();

		System.out.print("\nTask2 Kicked Off\n");
		Task2 task2 = new Task2();
		Thread task2Thread = new Thread(task2);
		task2Thread.start();

		task1.join();
		task2Thread.join();

		System.out.print("\nTask3 Kicked Off\n");
		for(int i=301; i<=399; i++) {
			System.out.print(i + " ");
		}
		System.out.println("\nTask3 Done");
		System.out.println("\nMain Done");
		
	}

```

##### Snippet-6 Explained

It is important to note that *Task1* and *Task2* are still independent sub-tasks. The thread scheduler is free to interleave their executions. However, *Task3* is kicked off only after both of them terminate.

#### Summary

In this step, we:

* Understood the need for thread communication
* Learned that Java provides mechanisms for threads to wait for each other
* Observed how the ```join()``` method can be used to sequence thread execution
  
### Step 07: ```synchronized``` Methods, And ```Thread``` Utilities

When a thread gets tired, you can put it to bed. Heck, you can do it even when it's fresh and raring to go! It's under your control, remember? 

The ```Thread``` class provides a couple of methods:
* ```public static native void sleep(int millis)``` : Calling this method will cause the thread in question, to go into a *blocked* / *waiting* state for **at least** ```millis``` milliseconds.
* ```public static native void yield()``` : Request thread scheduler to execute some other thread. The scheduler is free to ignore such requests.

##### Snippet-01 : Thread utilities

**jshell>** ```Thread.sleep(1000)```


**jshell>** ```Thread.sleep(10000)```


**jshell>**

##### Snippet-7 Explained

* ```Thread.sleep(1000)``` causes the ```JShell``` prompt to appear after a delay of *at least* 1 second. This delay is even more visible, when we execute ```Thread.sleep(10000)```.


### Step 08: Drawbacks of earlier approaches

We saw few of the methods for synchronization in the `Thread` class

* ```start()```
* ```join()```
* ```sleep()```
* ```wait()```


Above approaches have a few drawbacks:
* **No Fine-Grained Control**: Suppose, for instance , we want *Task3* to run after *any one* of *Task1* or *Task2* is done. How do we do it?
* **Difficult to maintain**: Imagine managing 5-10 threads with code written in earlier examples. It would become very difficult to maintain. 
* **NO Sub-Task Return Mechanism**: With the ```Thread``` ```class``` or the ```Runnable``` ```interface```, there is no way to get the result from a sub-task.

### Step 09: Introducing ```ExecutorService```

In order to address the serious limitations of the ```Thread``` API, a new ```Executor Service``` was added in **Java SE 5**. 

The ```ExecutorService``` is a framework you can use to manage threads in your code, better.  It has built-in ways to:
* Create and launch threads more intuitively
* Manage thread state and its life-cycle more easily
* Synchronize between threads with more control, and
* Handle groups of threads neatly

The ```ExecutorService``` provides utilities to achieve each one of these. Let's start with thread creation, which the next example takes care of.

##### Snippet-01 : Creating a Thread

**_ExecutorServiceRunner.java_**

```java

	import java.util.concurrent.ExecutorService;
	import java.util.concurrent.Executors;
	
	class Task1 extends Thread {
		public void run() {
			System.out.println("Task1 Started ");
			for(int i=101; i<=199; i++) {
				System.out.print(i + " ");
			}
			System.out.println("\nTask1 Done");
		}
	}

	class Task2 implements Runnable {
		@Override
		public void run() {
			System.out.println("Task2 Started ");
			for(int i=201; i<=299; i++) {
				System.out.print(i + " ");
			}
			System.out.println("\nTask2 Done");
		}
	}


	public class ExecutorServiceRunner {
		public static void main(String[] args) {
			ExecutorService executorService = Executors.newSingleThreadExecutor();
			executorService.execute(new Task1());
			executorService.execute(new Thread(new Task2()));
		}
	}

```

**_Console Output_**

_Task1 Started_

_101 102 103 104 105 106 107 108 109 110 111 112 113 114 115 116 117 118 119 120 121 122 123 124 125 126 127 128 129 130 131 132 133 134 135 136 137 138 139 140 141 142 143 144 145 146 147 148 149 150 151 152 153 154 155 156 157 158 159 160 161 162 163 164 165 166 167 168 169 170 171 172 173 174 175 176 177 178 179 180 181 182 183 184 185 186 187 188 189 190 191 192 193 194 195 196 197 198 199_

_Task1 Done_

_Task2 Started_

_201 202 203 204 205 206 207 208 209 210 211 212 213 214 215 216 217 218 219 220 221 222 223 224 225 226 227 228 229 230 231 232 233 234 235 236 237 238 239 240 241 242 243 244 245 246 247 248 249 250 251 252 253 254 255 256 257 258 259 260 261 262 263 264 265 266 267 268 269 270 271 272 273 274 275 276 277 278 279 280 281 282 283 284 285 286 287 288 289 290 291 292 293 294 295 296 297 298 299_

_Task2 Done_

##### Snippet-01 Explained

`ExecutorService executorService = Executors.newSingleThreadExecutor()` creates a single threaded executor service. That's why the tasks ran serially, one after the other.

##### Snippet-02 : main runs in parallel

Let's update the main method to do more things:

```java

	public class ExecutorServiceRunner {
		public static void main(String[] args) {
			ExecutorService executorService = Executors.newSingleThreadExecutor();
			executorService.execute(new Task1());
			executorService.execute(new Thread(new Task2()));
			System.out.print("\nTask3 Kicked Off\n");

			for(int i=301; i<=399; i++) {
				System.out.print(i + " ");
			}
			System.out.println("\nTask3 Done");
			System.out.println("\nMain Done");
			executorService.shutdown();
		}
	}

```

**_Console Output_**
```
Task1 Started 
101 
Task3 Kicked Off
102 301 103 302 104 303 105 304 106 305 107 306 108 109 110 111 112 307 113 114 115 116 117 118 119 120 121 122 308 123 309 124 310 125 311 126 312 127 313 128 314 129 315 130 316 131 132 133 317 134 318 135 319 136 137 320 138 321 139 322 140 323 141 324 142 325 143 326 144 145 146 147 327 148 149 328 150 329 151 330 152 331 332 153 154 155 156 157 158 159 160 161 162 163 164 165 166 167 168 169 170 171 172 173 174 175 176 177 178 179 180 181 182 183 184 185 186 187 188 189 190 191 192 193 333 194 334 195 196 335 197 198 199 336 
Task1 Done
337 338 Task2 Started 
339 201 202 203 204 205 206 207 340 341 208 209 210 211 212 213 214 215 216 217 218 219 220 342 221 222 223 224 225 343 344 226 345 346 227 347 348 228 229 349 230 231 232 350 233 351 352 234 235 236 237 238 239 240 241 242 243 244 245 246 247 248 353 249 354 355 250 356 251 357 358 252 359 253 360 254 255 361 256 362 363 257 258 364 259 365 366 260 367 261 262 368 263 264 369 265 370 266 371 267 268 372 269 373 270 374 375 271 376 377 272 378 273 379 274 380 275 381 382 276 277 383 278 384 279 385 386 280 387 281 388 282 283 284 389 285 390 391 286 392 287 393 288 394 395 289 396 290 397 291 398 399 
Task3 Done

Main Done
292 293 294 295 296 297 298 299 
Task2 Done

```

##### Snippet-02 Explained

The only order that we see in the resulting chaos is: *Task2* starts execution only after *Task1* is done.

Threads managed by `ExecutorService` run in parallel with `main` method.

### Step 10: Executor -  Customizing Number Of Threads

With the ```ExecutorService```, it is possible to create a *pool* of threads.

The following examples will show you how you can create thread pools of varying kinds, and of course, of different sizes. 

##### Snippet-03 : Executors for Concurrent threads

**_ExecutorServiceRunner.java_**

```java

	public class ExecutorServiceRunner {
		public static void main(String[] args) {
			ExecutorService executorService = Executors.newFixedThreadPool(2);
			executorService.execute(new Task1());
			executorService.execute(new Thread(new Task2()));
			System.out.print("\nTask3 Kicked Off\n");

			for(int i=301; i<=399; i++) {
				System.out.print(i + " ");
			}
			System.out.println("\nTask3 Done");
			System.out.println("\nMain Done");
			executorService.shutdown();
		}
	}

```

**_Console Output_**
```
Task1 Started 

Task3 Kicked Off
301 101 302 303 304 Task2 Started 
305 306 102 307 103 201 104 308 105 202 106 309 107 203 108 310 109 110 204 111 311 112 205 113 312 114 206 115 313 116 207 117 314 118 208 119 315 120 209 121 316 122 210 317 211 123 212 318 213 124 125 126 127 128 319 129 320 214 321 130 322 215 323 131 324 216 325 132 326 217 327 133 328 218 329 134 330 219 331 135 332 220 333 136 334 221 335 137 336 222 337 138 338 223 339 139 340 224 341 140 342 225 343 141 344 226 345 142 346 227 347 143 228 348 144 349 229 350 145 351 230 352 146 353 231 354 147 355 232 356 148 357 233 358 149 359 234 360 150 361 235 362 151 363 236 364 237 152 238 365 239 153 240 366 241 242 154 243 367 244 155 245 368 246 156 157 247 369 248 158 249 370 250 159 251 252 253 254 255 256 257 258 259 260 261 262 263 264 265 266 371 267 372 373 374 268 269 160 270 271 375 376 377 378 379 380 381 382 383 161 162 163 164 165 384 166 272 167 168 169 170 171 172 385 173 386 387 273 388 389 174 390 391 274 392 175 393 275 394 176 395 276 396 397 177 398 178 179 277 180 399 181 278 182 183 
Task3 Done

Main Done
184 185 279 280 281 282 283 186 187 284 285 286 188 189 287 288 289 190 191 290 192 291 193 292 194 293 195 196 294 295 296 297 298 299 
Task2 Done
197 198 199 
Task1 Done
```

##### Snippet-03 Explained

We created `ExecutorService` by using `Executors.newFixedThreadPool(2)`. So, 'ExecutorService` uses two parallel threads at a maximum.
* *Task1* and *Task2* execute concurrently as part of the ```ExecutorService```, and
* The thread running ```main()``` executes concurrently with this thread pool, created by ```ExecutorService```.


##### Snippet-04 : All-Executor Task Execution

Let's create a simple example to allow to play with 'ExecutorService'.

**_ExecutorServiceRunner.java_**

```java

	import java.util.concurrent.ExecutorService;
	import java.util.concurrent.Executors;

	class Task extends Thread {
		private int number;
		
		public Task(int number) {
			this.number = number;
		}

		public void run() {
			System.out.println("Task " + number + " Started");
			for(int i=number*100; i<=number*100+99; i++) {
				System.out.print(i + " ");
			}
			System.out.println("\nTask " + number +" Done");
		}
	}

	public class ExecutorServiceRunner {
		public static void main(String[] args) {
			ExecutorService executorService = Executors.newFixedThreadPool(2);
			executorService.execute(new Task(1));
			executorService.execute(new Task(2));
			executorService.execute(new Task(3));
			executorService.shutdown();
		}
	}

```


##### Snippet-04 Explained

`Executors.newFixedThreadPool(2)` - Two threads in parallel. The thread ```new Task(3)``` is executed only after any one of ```new Task(1)``` and ```new Task(2)``` have completed their execution.

##### Snippet-04 : Larger Thread Pool Size

```java

	public class ExecutorServiceRunner {
		public static void main(String[] args) {
			ExecutorService executorService = Executors.newFixedThreadPool(3);
			executorService.execute(new Task(1));
			executorService.execute(new Task(2));
			executorService.execute(new Task(3));
			executorService.execute(new Task(4));
			executorService.execute(new Task(5));
			executorService.execute(new Task(6));
			executorService.execute(new Task(7));
			executorService.shutdown();
		}
	}

```


##### Snippet-04 Explained

We made the pool larger to 3:
* Initially Tasks ```1```, ```2```, ```3``` are added to the ```ExecutorService``` and are started.
* As soon as any one of them is terminated, another task from the thread pool is picked up for execution, and so on. A classic case of musical chairs, with regard to the slots available in the ```ExecutorService``` thread pool, is played out here!

#### Summary

In this step, we:

* Explored how one can create pools of threads of the same kind, using the ```ExecutorService```
* Noted how one could specify the size of a thread pool

### Step 11:  ```ExecutorService```: Returning Values From Tasks

##### Snippet-01: Returning a Future Object

So far, we have only seen sub-tasks that are largely independent, and which don't return any result to the main program that launched them.

To be able to return a value from a Thread, Java provides a ```Callable<T>``` interface.

The next example tells you how to implement ```Callable<T>```, and use it with ```ExecutorService```.

**_ExecutorServiceRunner.java_**

```java

	import java.util.concurrent.ExecutorService;
	import java.util.concurrent.Executors;
	import java.util.concurrent.Callable;

	class CallableTask implements Callable<String> {
		private String name;

		public CallableTask(String name) {
			this.name = name;
		}

		@Override
		public String call() throws Exception {
			Thread.sleep(1000);
			return "Hello " + name;
		}
	}

	public class CallableRunner {
		public static void main(String[] args) throws InterruptedException, ExecutionException {
			ExecutorService executorService = Executors.newFixedThreadPool(1);
			Future<String> welcomeFuture = executorService.submit(new CallableTask("amitsa"));
			System.out.println("CallableTask amitsa Submitted");
			String welcomeMessage = welcomeFuture.get();
			System.out.println(welcomeMessage);
			executorService.shutdown();
		}
	}

```

**_Console Output_**

_CallableTask amitsa Submitted_

_Hello amitsa_

##### Snippet-01 Explained

- `class CallableTask implements Callable<String>` - Implement `Callable`. Return type is `String`
- `public String call() throws Exception {` - Implement `call` method and return a `String` value back.
-  ```executorService.submit(new CallableTask(String))``` adds a callable task to its thread pool. This puts the task thread in a **RUNNABLE** state. Subsequently, the program goes ahead to invoke its ```call()``` method.
- `welcomeFuture.get()` - Ensures that the `main` thread waits for the result of the operation. 


#### Summary

In this step, we:

* Understood the need for a mechanism, to create sub-tasks that return values
* Discovered that Java has a ```Callable<T>``` interface to do exactly this
* Saw that the ```ExecutorService``` is capable of managing ```Callable``` threads

### Step 11: Executors - Waiting For Many Callable Tasks To Complete

```ExecutorService``` framework also allows you to create a pool of ```Callable``` threads. Not only that, you can collect their return values together.

##### Snippet-01 : Waiting for Multiple ```Callable``` Threads 

**_MultipleCallableRunner.java_**

```java

	import java.util.concurrent.ExecutorService;
	import java.util.concurrent.Executors;
	import java.util.concurrent.Callable;

	class CallableTask implements Callable<String> {
		private String name;

		public CallableTask(String name) {
			this.name = name;
		}

		@Override
		public String call() throws Exception {
			Thread.sleep(1000);
			return "Hello " + name;
		}
	}

	public class MultipleCallableRunner {
		public static void main(String[] args) throws InterruptedException, ExecutionException {
			ExecutorService executorService = Executors.newFixedThreadPool(1);
			List<CallableTask> tasks = List.of(new CallableTask("amitsa"),
												new CallableTask("Ranga"),
												new CallableTask("Adam"));
			List<Future<String>> welcomeAll = executorService.invokeAll(tasks);
			for(Future<String> welcomeFuture : welcomeAll) {
				System.out.println(welcomeFuture.get());
			}
			executorService.shutdown();
		}
	}

```

**_Console Output_**

_Hello amitsa_

_Hello Ranga_

_Hello Adam_

##### Snippet-01 Explained

The ```invokeAll()``` method of  ```ExecutorService``` allows for a list of ```Callable``` tasks to be launched in the thread pool. Also, a ```List``` of ```Future``` objects can be used to hold return values, one for each such ```Callable``` thread.

The list of return values can be accessed only after **all** the threads are done, and have returned their results. 

This can be verified from the console output. all the planned welcome messages are printed in one go, but only after a wait of at least ```3000``` milliseconds has been completed.

Let's now see what scenario would pan out with a larger thread pool size. 

##### Snippet-02 : List of Callable tasks with larger thread pool

```java

	public class MultipleCallableRunner {
		public static void main(String[] args) throws InterruptedException, ExecutionException {
			ExecutorService executorService = Executors.newFixedThreadPool(3);
			List<CallableTask> tasks = List.of(new CallableTask("amitsa"),
												new CallableTask("Ranga"),
												new CallableTask("Adam"));
			List<Future<String>> welcomeAll = executorService.invokeAll(tasks);
			for(Future<String> welcomeFuture : welcomeAll) {
				System.out.println(welcomeFuture.get());
			}
			executorService.shutdown();
		}
	}

```

**_Console Output_**

_Hello amitsa_

_Hello Ranga_

_Hello Adam_

##### Snippet-02 Explained

The welcome messages all get printed in a batch again, but their collective wait gets shorter. This is because:
* The thread pool size is now ```3```, not ```2``` as earlier. This means all the three tasks can be put in the **RUNNABLE** state at once. 
* Then, they go into their **BLOCKED** state also almost simultaneously, which means their collective wait time is much less than ```3000``` milliseconds. That's one advantage of a larger thread pool!

#### Summary 

In this step, we:

* Learned that it's possible to collect the return values of a pool of ```Callable``` threads, at one go.
* This is done using the ```invokeAll()``` method for their launch, and specifying a ```List``` of ```Future``` objects to hold these results
* Changing the thread pool size for such scenarios can change response time dramatically

### Step 12:  Executor - Wait Only For The Fastest Task

Let's look at how you can wait for any of the three tasks to complete.

##### Snippet-01 : Wait only for fastest

```java

	public class MultipleAnyCallableRunner {
		public static void main(String[] args) throws InterruptedException, ExecutionException {
			ExecutorService executorService = Executors.newFixedThreadPool(3);
			List<CallableTask> tasks = List.of(new CallableTask("amitsa"),
												new CallableTask("Ranga"),
												new CallableTask("Adam"));
			String welcomeMessage = executorService.invokeAny(tasks);
			System.out.println(welcomeMessage);
			executorService.shutdown();
		}
	}

```

**_Console Output_**

_Hello Ranga_

**_Console Output_**

_Hello amitsa_

**_Console Output_**

_Hello Ranga_

**_Console Output_**

_Hello Adam_

##### Snippet-01 Explained

The method ```invokeAny()``` returns when the first of the sub-tasks is done. Also, the returned value is not a ```Future``` object. It's the return value of the ```call()``` method.

We can see that over different executions, the order of console output changes. This is because: 
* All three tasks are created together, in a thread pool of size ```3```. 
* Therefore, these are independent threads, going into their **RUNNABLE** states almost at once. 

#### Summary

In this step, we:

* Learned that ```ExecutorService``` has a way to return the first result, from a poll of ```Callable``` threads

## Introduction To Exception handling


Recommended Exception handling Videos
- https://www.youtube.com/watch?v=34ttwuxHtAE

There are two kinds of errors a programmer faces:

* **Compile-time** Errors: Flagged by the compiler when it detects syntax or semantic errors.
* **Run-time** Errors: Detected by the run-time environment when executing code

Example runtime errors include:
* Running out of *heap-memory* for objects, or space for the method *call stack*
* Dividing a number by ```0```
* Trying to *read* from an *unopened* file

Exceptions are *unexpected* conditions; their occurrence does not make a programmer bad (or good, for that matter). It is a programmer's responsibility to be *aware* of potential exceptional conditions in her program, and use a mechanism to *handle* them effectively.

Handling an exception generally involves two important aims:
1. Provide a useful message to the end user.
2. Log enough information to help a programmer identify root cause.

### Step 01: Introducing Exceptions

In the previous step, we gave you a few instances of exceptions, such as your program running out of memory, or your code trying to divide a number by ```0```. 

Want to see a live example of the Java run-time throwing an exception? The next example will satisfy your thirst.

##### Snippet-1 : Exception Condition

**_ExceptionHandlingRunner.java_**

```java

	package com.amitsa.exceptionhandling;
	
	public class ExceptionHandlingRunner {
		public static void main(String[] args) {
			callMethod();
			System.out.println("callMethod Done");		
		}

		static void callMethod() {
			String str = null;
			int len = str.length();
			System.out.println("String Length Done");
		}		
	}	

```

**_Console Output_**

**_java.lang.NullPointerException_**

_Exception in thread "main" java.lang.NullPointerException_

_at com.amitsa.exceptionhandling.ExceptionHandlingRunner.callMethod (ExceptionHandlingRunner.java:8)_

_at com.amitsa.exceptionhandling.ExceptionHandlingRunner.main (ExceptionHandlingRunner.java:4)_


##### Snippet-01 Explained

A ```java.lang.NullPointerException``` is **thrown** by the Java run-time when we called ```length()``` on the `null` reference.

If an exception is not handled in the entire call chain, including `main`, the exception is thrown out and the program terminates. ```System.out.println()``` statements after the exception are never executed. 

The runtime prints the **call stack trace** onto the console. 

For instance, consider this simple ```class``` ```Test```:

```java

	public class Test {
		public static void main(String[] args) {
			callOne();
		}

		public static void callOne() {
			callTwo();
		}

		public static void callTwo() {
			callThree();
		}

		public static void callThree() {
			String name;
			System.out.println("This name is %d characters long", name.length());
		}
	}

``` 

However, the code name.length() used in ```callThree()``` caused a ```NullPointerException``` to be thrown. However, this is  not handled, and the console coughs up the following display:
		
_Exception in thread "main" java.lang.NullPointerException_

_at Test.callThree(Test.java:13)_

_at Test.callTwo(Test.java:9)_

_at Test.callOne(Test.java:6)_

_at Test.main(Test.java:3)_

This is nothing but a call trace of the stack, *frozen in time*.

#### Summary

In this step, we:

* Saw a live example of an exception being thrown
* Understood how a call trace on the stack appears when a exception occurs
* Reinforced this understanding with another example

### Step 02: Handling An Exception

In Java, exception handling is achieved through a **```try```-```catch```** *block*. 

##### Snippet-01 : Handling An Exception

**_ExceptionHandlingRunner.java_**

```java

	package com.amitsa.exceptionhandling;
	
	public class ExceptionHandlingRunner {
		public static void main(String[] args) {
			method1();
			System.out.println("main() Done");
		}
		
		static void method1() {
			method2();
			System.out.println("method1() done");
		}

		static void method2() {
			try {
				String str = null;
				int len = str.length();
				System.out.println("method2() Done");
			} catch (Exception ex) {
			}
		}
	}

```

**_Console Output_**

_method1() Done_

_main() Done_

##### Snippet-01 Explained

We have handled an exception here with a ```try```-```catch``` block. Its syntax resembles this:

```java

	try {
		//< program-logic code block >
	} catch (Exception e) {		
		//< exception-handling code block >
	}

```

The exception (the ```NullPointerException```) still occurs after adding this block, but now it's actually **caught**. Although we did nothing with the caught exception, we did avoid a sudden end of the program.

The statements following  ```int len = str.length()``` in `method2` are not executed. 

However, all of ```method1()```'s code was run (with the ```"method1 Done"``` message getting printed). ```main()``` method also completed
