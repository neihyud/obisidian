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
