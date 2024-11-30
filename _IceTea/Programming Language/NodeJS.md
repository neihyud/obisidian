# How NodeJS works
 - **Non block I/O:**
	 - nếu không có data ở thời điểm hiện tại, hàm sẽ trả về một biến constant được xác định trước và cho biết không có data ở thời điểm đó
- **Event demultiplexing**:
	- 
	## Reactor Pattern
- The main idea behind the reactor pattern is to have a **handler** associated with each I/O operation. A handler in Node.js is represented by a callback (or cb for short) function.
- **The handler** will be invoked as soon as an event is produced and processed by the event loop
![[Pasted image 20241014234542.png]]

1. The application generates a new I/O operation by submitting a request to
the Event Demultiplexer. The application also specifies a handler, which
will be invoked when the operation completes. Submitting a new request
to the Event Demultiplexer is a non-blocking call and it immediately returns
control to the application.
2. When a set of I/O operations completes, the Event Demultiplexer pushes
a set of corresponding events into the Event Queue.
3. At this point, the Event Loop iterates over the items of the Event Queue.
4. For each event, the associated handler is invoked.
5. The handler, which is part of the application code, gives back control
to the Event Loop when its execution completes (5a). While the handler
executes, it can request new asynchronous operations (5b), causing new
items to be added to the Event Demultiplexer (1).
6. When all the items in the Event Queue are processed, the Event Loop blocks
again on the Event Demultiplexer, which then triggers another cycle when
a new event is available.
## Libuv
- a higher-level abstraction to be built for the **event demultiplexer**.  => objective to make Node.js compatible with all the major operating systems and normalize the non-blocking behavior of the different types of resource.
## The recipe for Node.js
![[Pasted image 20241020233106.png]]
# Module System
- The dynamic import happens asynchronously, so we can use the .then() hook on the returned promise to get notified when the module is ready to be used.
## Module Load
- **Phase 1** - Construction (or parsing): Find all the imports and recursively load the content of every module from the respective file. 
- **Phase 2** - Instantiation: For every exported entity, keep a named reference in memory, but don't assign any value just yet. Also, references are created for all the import and export statements tracking the dependency relationship between them (linking). No JavaScript code has been executed at this stage. 
- **Phase 3** - Evaluation: Node.js finally executes the code so that all the previously instantiated entities can get an actual value. Now running the code from the entry point is possible because all the blanks have been filled.

=> 
**Phase 1**: is about finding all the dots, 
**Phase 2**: connects those creating paths, and, finally
**Phase 3**: walks through the paths in the right order.


:::error
- CommonJS will execute all the files while the dependency graph is explored => can import in **if statements** or **for loop**



# Gzip use stream
```nodejs=
	// gzip-stream.js
	import { createReadStream, createWriteStream } from 'fs'
	import { createGzip } from 'zlib'
	
	const filename = process.argv[2]
	
	createReadStream(filename)
		.pipe(createGzip())
		.pipe(createWriteStream(`${filename}.gz`))
		.on('finish', () => console.log('File successfully compressed'))
```

# Even loop in JS
https://www.lydiahallie.com/blog/event-loop
https://200lab.io/blog/event-loop-la-gi/
![[even-loop.mp4]]
- Tiny component within the JS runtime
- **Role**: handling of asynchronous tasks
- Single thread: working with a single `Call Stack`
## Call Stack
 ![[call-stack.mp4]]
- `Call Stack`: manages the execution of our program
	- when invoke function, a execution context push onto the `Call stack` => The top-most function in the call stack is evaluated, which could in turn invoke another function, and so on. An execution context is popped off the `Call Stack` when the function completed its execution.

=>   JavaScript is only able to handle **one task at a time**; if one task is taking too long to pop off, no other tasks can get handled.

![[long-task.mp4]]
## Web APIs
- **bridge** between the JavaScript runtime and the browser features,
- Some `Web APIs` allow us to initiate async tasks, and **offload longer-running tasks** to the browser!
		- Invoking a method exposed by such an API is really just to **offload the longer-running task to the browser environment**, and **set up handlers to handle the eventual completion of this task.**

![[offload-longer-running.mp4]]
	- After initiating the async task (without waiting for the result), the execution context can quickly get popped off the `Call Stack`; it's non-blocking! 
###  Callback-based APIs 
![[Pasted image 20241130122729.png]]

![[geo-call.mp4]]

![[geo-add-task-queue.mp4]]

![[add-task-queue.mp4]]
## Task Queue  (Callback Queue)
- **Task queue** holds Web API callbacks and event handlers to be executed at some point in the future
## Even loop
- `Event Loop` to continuously check if the `Call Stack` is empty. Whenever the `Call Stack` is empty — meaning there are **no currently running tasks**. It takes the first available task from the `Task Queue` and moves this onto the `Call Stack`
![[taskqueue-to-callback.mp4]]

Ex: **Set Time out**
![[ExSetTimeOut.mp4]]

## Microtask Queue
- The `Microtask Queue` is another queue in the runtime with a **higher priority** than the `Task Queue`. Include
	- Promise handle callback
	- aysn/await
	- `MutationObserver` callbacks
	- `queueMicrotask` callbacks
- When the `Call Stack` is empty, the `Event Loop` first processes **_all_ microtasks** from the `Microtask Queue` before moving on to the `Task Queue`.

![[fetch.mp4]]

![[fetch2.mp4]]

![[fetch3.mp4]]

![[fetch4.mp4]]

## Recap
-

## How does Event Loop work in JavaScript?
The Event loop has two main components: the Call stack and the Callback queue.
### Call Stack
- The Call stack is a data structure that stores the tasks that need to be executed. It is a LIFO (Last In, First Out) data structure, which means that the last task that was added to the Call stack will be the first one to be executed
### Callback Queue
- The Callback queue is a data structure that stores the tasks that have been completed and are ready to be executed. It is a FIFO (First In, First Out) data structure, which means that the first task that was added to the Callback queue will be the first one to be executed.
### Event Loop's Workflow:
1. Executes tasks from the Call Stack.
2. For an asynchronous task, such as a timer, it runs in the background. JavaScript proceeds to the next task without waiting.
3. When the asynchronous task concludes, its callback function is added to the Callback Queue.
4. If the Call Stack is empty and there are tasks in the Callback Queue, the Event Loop transfers the first task from the Queue to the Call Stack for execution.


---
Order priorities of `process.nextTick`, `Promise`, `setTimeout` and `setImmediate` are as follows:

1. `process.nextTick`: Highest priority, executed immediately after the current event loop cycle, before any other I/O events or timers.
2. `Promise`: Executed in the microtask queue, after the current event loop cycle, but before the next one.
3. `setTimeout`: Executed in the timer queue, after the current event loop cycle, with a minimum delay specified in milliseconds.
4. `setImmediate`: Executed in the check queue, but its order may vary based on the system and load. It generally runs in the next iteration of the event loop after I/O events.

### Npx 
`npx` is a tool that allows you to run Node.js packages **without** installing them. It is used to execute Node.js packages that are not installed globally.

# Even loop in NodeJS