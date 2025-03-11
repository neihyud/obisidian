	# How NodeJS works
 - **Non block I/O:**
	 - nếu không có data ở thời điểm hiện tại, hàm sẽ trả về một biến constant được xác định trước và cho biết không có data ở thời điểm đó
- **Event demultiplexing**:
	- 
	## Reactor Pattern
- The main idea behind the reactor pattern is to have a **handler** associated with each I/O operation. A handler in Node.js is represented by a callback (or cb for short) function.
- **The handler** will be invoked as soon as an event is produced and processed by the event loop
![[even_loop.png]]

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

# Even loop in JS (Web)
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
- Call Stack là một **ngăn xếp (stack)** dùng để **ghi nhớ thứ tự lời gọi hàm** trong chương trình. Khi một hàm được gọi, nó sẽ được **đẩy vào Call Stack**, và khi hàm hoàn thành, nó sẽ **bị loại bỏ (popped) khỏi Call Stack**.
- **Node.js là single-threaded**, nghĩa là nó chỉ có **một Call Stack** để xử lý các tác vụ. Nhưng Node.js có thể xử lý **bất đồng bộ (async)** nhờ **Event Loop**.
## **2. Cách hoạt động của Event Loop**

👉 Event Loop có nhiệm vụ liên tục **kiểm tra Call Stack** và **chuyển các tác vụ cần xử lý** vào đó. Nó hoạt động theo 6 bước chính:

### **🛠 1. Call Stack (Ngăn xếp hàm)**

- Khi một hàm được gọi, nó sẽ được đưa vào **Call Stack** theo **LIFO (Last In, First Out)**.
- Nếu hàm là **đồng bộ**, nó chạy ngay lập tức.
- Nếu hàm là **bất đồng bộ** (như `setTimeout`, I/O), nó sẽ được gửi đến **Web APIs**.

---

### **🛠 2. Web APIs (Xử lý tác vụ bất đồng bộ)**

- Các tác vụ bất đồng bộ như `setTimeout`, `fetch()`, `fs.readFile()` sẽ được xử lý bởi **Web APIs** của trình duyệt hoặc Node.js.
- Sau khi hoàn thành, kết quả của chúng được đẩy vào **Callback Queue** hoặc **Microtask Queue**.

---

### **🛠 3. Callback Queue (Hàng đợi nhiệm vụ)**

- Chứa các **callback** từ Web APIs cần được thực thi.
- Các hàm từ `setTimeout()`, `setInterval()` và I/O sẽ vào hàng đợi này.

---

### **🛠 4. Microtask Queue (Hàng đợi ưu tiên cao)**

- Chứa các **Promises (resolve, reject) và process.nextTick()**.
- Luôn được xử lý trước khi đến **Callback Queue**.

---

### **🛠 5. Event Loop**

- Kiểm tra **Call Stack** rỗng hay không.
- Nếu Call Stack rỗng:  
    🔹 Lấy **Microtask Queue** trước → thực thi tất cả promises.  
    🔹 Sau đó mới lấy **Callback Queue** → thực thi các tác vụ như `setTimeout`.
- Nếu Call Stack không rỗng: Chờ cho đến khi rỗng rồi mới tiếp tục.

---

### **🛠 6. Call Stack hoàn tất**

- Khi tất cả các nhiệm vụ đã hoàn thành, Call Stack trống → Node.js tiếp tục vòng lặp mới.

---
### 📌 **1. Node.js có phải đơn luồng không?**

✅ **Đúng!** Node.js hoạt động trên **một luồng chính (single-threaded)** để thực thi JavaScript.

📌 **Nhưng…**

- **Node.js vẫn có thể xử lý nhiều tác vụ cùng lúc nhờ cơ chế bất đồng bộ (asynchronous)**.
- Nó **không thực sự chạy song song trên main thread**, nhưng vẫn có thể **thực thi đa luồng ở phía sau** nhờ vào **libuv**.
- **Các tác vụ I/O (đọc/ghi file, gọi API, truy vấn database,...)** được xử lý **song song** bằng **Thread Pool** của libuv.


---

## 🔹 **1. Main Thread là gì?**

📌 **Main Thread (luồng chính) trong Node.js** là luồng chạy **Event Loop**, nơi tất cả mã JavaScript chính được thực thi.

### 🛠 **Đặc điểm của Main Thread:**

1. **Chạy đơn luồng**:
    
    - Node.js mặc định chỉ chạy JavaScript trên một luồng duy nhất (Main Thread).
2. **Quản lý Event Loop**:
    
    - Main Thread chịu trách nhiệm xử lý **Event Loop**, giúp điều phối các tác vụ **bất đồng bộ** (I/O, network requests, timers, promises...).
3. **Không phù hợp với CPU-heavy tasks**:
    
    - Vì Main Thread chỉ có một luồng, nên nếu một tác vụ **quá nặng về CPU** (ví dụ: vòng lặp lớn hoặc tính toán phức tạp), nó có thể **chặn Event Loop**, khiến ứng dụng bị "đóng băng" (không phản hồi).
4. **Phù hợp với I/O-heavy tasks**:
    
    - Do Node.js sử dụng **mô hình bất đồng bộ và non-blocking**, Main Thread rất hiệu quả khi xử lý các tác vụ **I/O-heavy** (như đọc ghi file, gọi API, truy vấn database).

📌 **Ví dụ về Main Thread chạy đơn luồng**

javascript

CopyEdit

`console.log("1️⃣ Start");  setTimeout(() => {     console.log("2️⃣ Async Task Done"); }, 2000);  console.log("3️⃣ Continue Running");`

**Kết quả in ra:**

sql

CopyEdit

`1️⃣ Start 3️⃣ Continue Running 2️⃣ Async Task Done (sau 2 giây)`

🔍 **Giải thích:**

- `setTimeout()` là **tác vụ bất đồng bộ**, nên nó không chặn Main Thread.
- Main Thread **tiếp tục thực thi mã** mà không phải đợi `setTimeout()` hoàn thành.

---

## 🔹 **2. Worker Threads là gì?**

📌 **Worker Threads (luồng làm việc) trong Node.js** là các **luồng phụ** được tạo ra để **xử lý tác vụ nặng về CPU** mà không làm chậm Main Thread.

### 🛠 **Đặc điểm của Worker Threads:**

1. **Chạy song song với Main Thread**:
    
    - Worker Threads chạy trong **các luồng riêng biệt** và không làm ảnh hưởng đến Event Loop.
2. **Phù hợp với CPU-heavy tasks**:
    - Khi gặp tác vụ nặng như **xử lý ảnh, mã hóa dữ liệu, tính toán phức tạp**, chúng ta có thể sử dụng Worker Threads để thực thi tác vụ song song mà không làm chậm Main Thread.
3. **Không chia sẻ bộ nhớ trực tiếp với Main Thread**:
    - Worker Threads không thể truy cập trực tiếp vào **biến của Main Thread**. Thay vào đó, chúng **giao tiếp qua message-passing**.

📌 **Ví dụ về Worker Thread xử lý tính toán lớn**

javascript

CopyEdit

``const { Worker, isMainThread, parentPort } = require("worker_threads");  if (isMainThread) {     console.log("📌 Main Thread: Khởi động Worker...");     const worker = new Worker(__filename); // Tạo Worker từ chính file này      worker.on("message", (message) => {         console.log(`🔹 Worker trả về: ${message}`);     }); } else {     console.log("🔹 Worker Thread: Bắt đầu tính toán...");     let sum = 0;     for (let i = 0; i < 1e9; i++) {         sum += i;     }     parentPort.postMessage(sum); }``

🔥 **Kết quả:**

less

CopyEdit

`📌 Main Thread: Khởi động Worker... 🔹 Worker Thread: Bắt đầu tính toán... 🔹 Worker trả về: 499999999500000000`

🔍 **Giải thích:**

- **Worker Thread** thực hiện tính toán lớn mà **không chặn Main Thread**.
- **Main Thread** có thể tiếp tục xử lý các yêu cầu khác trong khi Worker Thread đang chạy.

---

## 🔹 **3. So sánh Main Thread và Worker Threads**

|**Đặc điểm**|**Main Thread**|**Worker Threads**|
|---|---|---|
|**Số lượng**|1 duy nhất|Có thể tạo nhiều Worker Threads|
|**Vai trò chính**|Xử lý JavaScript chính, chạy Event Loop|Xử lý tác vụ CPU-heavy song song|
|**Bộ nhớ chia sẻ?**|Không (có thể chia sẻ qua message-passing)|Không (mỗi Worker có bộ nhớ riêng)|
|**Xử lý I/O tốt?**|✅ Rất tốt (non-blocking)|❌ Không tối ưu (không cần thiết)|
|**Xử lý CPU-heavy tốt?**|❌ Không tốt (chặn Event Loop)|✅ Rất tốt (xử lý song song)|

---

## 🔹 **4. Khi nào dùng Main Thread, Thread Pool và Worker Threads?**

📌 **Dùng Main Thread khi:**  
✅ Xử lý **JavaScript logic chính**.  
✅ Gửi HTTP request (API fetch, database query).  
✅ Đọc/Ghi file nhỏ (I/O nhẹ).

📌 **Dùng Thread Pool khi:**  
✅ Tác vụ **I/O-heavy** như truy vấn database, mã hóa dữ liệu.  
✅ Sử dụng `fs.readFile()`, `crypto.pbkdf2()`, `zlib.gzip()`, `dns.lookup()`.

📌 **Dùng Worker Threads khi:**  
✅ Cần xử lý **CPU-heavy** như tính toán số lớn, machine learning, xử lý ảnh/video.  
✅ Khi cần tận dụng **đa luồng để tăng tốc** trong Node.js.

---

**Worker Threads trong Node.js không có cơ chế Event Loop riêng** như Main Thread.

---

### 🛠 **1. Event Loop có trong Main Thread nhưng không có trong Worker Threads**

- Trong Node.js, **Main Thread** chạy trên **Event Loop**, giúp xử lý các tác vụ **bất đồng bộ** như I/O, timers, promises, v.v.
- **Worker Threads** chạy trên **luồng riêng biệt** và **không có Event Loop**. Điều này có nghĩa là một Worker Thread không tự động chạy các tác vụ non-blocking như Main Thread.

📌 **Main Thread có Event Loop:**

javascript

CopyEdit

`console.log("1️⃣ Start");  setTimeout(() => {     console.log("2️⃣ Async Task Done"); }, 2000);  console.log("3️⃣ Continue Running");`

🔹 **Kết quả:**

sql

CopyEdit

`1️⃣ Start 3️⃣ Continue Running 2️⃣ Async Task Done (sau 2 giây)`

👉 **Event Loop giúp setTimeout() chạy mà không chặn luồng chính.**

---

### ⚡ **2. Worker Threads hoạt động thế nào nếu không có Event Loop?**

- Worker Threads trong Node.js không có Event Loop, nhưng **vẫn có thể thực hiện các tác vụ bất đồng bộ nếu được lập trình cẩn thận.**
- Thay vì sử dụng Event Loop, Worker Threads nhận và xử lý dữ liệu qua **message-passing** (gửi và nhận thông điệp từ Main Thread).

📌 **Ví dụ về Worker Thread xử lý đồng bộ (không có Event Loop)**

javascript

CopyEdit

``const { Worker, isMainThread, parentPort } = require("worker_threads");  if (isMainThread) {     console.log("📌 Main Thread: Gửi task...");     const worker = new Worker(__filename);      worker.on("message", (message) => {         console.log(`🔹 Worker trả về: ${message}`);     });      worker.postMessage("Hello Worker!"); } else {     parentPort.on("message", (msg) => {         console.log(`🔹 Worker nhận: ${msg}`);         parentPort.postMessage("Task hoàn thành!");     }); }``

🔍 **Giải thích:**

- **Main Thread** gửi tin nhắn đến Worker Thread.
- **Worker Thread** xử lý và gửi phản hồi lại, nhưng **không có Event Loop** để tự động điều phối các tác vụ async như Main Thread.

---

### 🔥 **3. Có thể sử dụng async/await trong Worker Threads không?**

Có! Dù Worker Threads không có Event Loop, bạn vẫn có thể sử dụng **async/await** để thực thi tác vụ bất đồng bộ **bên trong Worker** nếu bạn gọi một tác vụ async thủ công.

📌 **Ví dụ sử dụng async/await trong Worker Thread**

javascript

CopyEdit

`const { Worker, isMainThread, parentPort } = require("worker_threads");  if (!isMainThread) {     async function fetchData() {         return new Promise((resolve) => {             setTimeout(() => {                 resolve("✅ Dữ liệu tải xong!");             }, 2000);         });     }      (async () => {         const data = await fetchData();         parentPort.postMessage(data);     })(); }`

🔍 **Giải thích:**

- **Không có Event Loop**, nên `setTimeout()` không tự động được xử lý như Main Thread.
- Nhưng nếu ta dùng **async/await**, Worker Thread vẫn có thể **chờ dữ liệu** và gửi kết quả khi hoàn thành.