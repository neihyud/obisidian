---
title: Nodejs And ExpressJs
tags: [Web, Backend]

---

###### tags: `Web` `Backend`
- NodeJS là đơn luồng vì nếu nó là đa luồng, khi sửa xóa cùng một element trên DOM sẽ bị conflic

# Bug

- can not set headers after they are send to the client -> bug do return về hai res.json 

#

- **Type moduel lấy path tuyệt đối**: lấy từ folder root, chú ý xem đã vào thư mục gốc chưa
-  10 MB dữ liệu văn bản chứa 10.000.000 ký tự (một ký tự trong UTF-8 chiếm một byte).
-  1MB = 1024Kb
-  For computers to understand, process, and store data, data has to be converted to binary code. This is mainly because the computer processor is made of transistors, electronic machines that are activated by on (1) and off (0) signals.
- npm ls: print dependencies in package
- biến trang web của bạn thành ứng dụng dành cho thiết bị di động với Cordova
-  res.setHeader("Content-Disposition", "attachment;filename= name_file.csv"); 
==> down file
- `nproc`: check core in my computer
- `time curl --get http://localhost:3000/blocking`: Lệnh timeđo thời gian curl lệnh chạy. Lệnh curl gửi yêu cầu HTTP đến URL đã cho và --gettùy chọn hướng dẫn curl thực hiện GET yêu cầu.
- nodejs single thread? chúng ta có thể chạy song song, nhưng ta không tạo thread và không đồng bộ hóa chúng. Máy ảo và OS sẽ chạy I/O song song và khi trở lại Javascript code, phần Javascript code sẽ chỉ chạy đơn luồng.
# Introduction
A Node.js app runs in a single process, without creating a new thread for every request. Node.js provides a set of asynchronous I/O primitives in its standard library that prevent JavaScript code from blocking and generally, libraries in Node.js are written using non-blocking paradigms, making blocking behavior the exception rather than the norm.

When Node.js performs an I/O operation, like reading from the network, accessing a database or the filesystem, instead of blocking the thread and wasting CPU cycles waiting, Node.js will resume the operations when the response comes back.

:::info
- NodeJS là open-source and cross-platform JS runtime environment
- NodeJS là single thread (V8 JS engine) và đa luồng (gửi request http, I/O database, file system, crypto, ...) 
- Khi thực thi một chương trình, JS sẽ thực thi từ trên xuống dưới, đối vs các tác vụ async thì sẽ  sử dụng evenloop để quản lý và lập lịch các sự kiện. Event Loop giữ cho mã JavaScript tiếp tục chạy bằng cách chờ đợi và xử lý các sự kiện theo cơ chế FIFO (First In, First Out). Khi một tác vụ bất đồng bộ hoàn thành, nó sẽ được đưa vào hàng đợi và Event Loop sẽ tiếp tục chạy các tác vụ trong hàng đợi này khi không có gì đang chờ xử lý.
:::

# Nodejs And ExpressJs

- Node.js is a popular open-source runtime environment that can execute JavaScript outside of the browser using the V8 JavaScript engine
- A collection of one or more modules is commonly referred to as a package, and these packages are themselves organized by package managers (NPM - nodejs package manager)
- Xây dựng app desktop nodejs bằng electron
- magic number: check ext file: https://en.wikipedia.org/wiki/Magic_number_(programming)#Magic_numbers_in_files

![](https://i.imgur.com/Jr0iZk1.png)

![](https://i.imgur.com/tO1DMgr.png)
![](https://i.imgur.com/Oa5hbvo.png)
![](https://i.imgur.com/35R10kE.png)
- libuv
![](https://i.imgur.com/Vx4r3th.png)

>Thông qua hình ta thấy libuv bao gồm các thành phần là: Event Queue, Event Loop và Threal Pool.
Nếu như ở các ngôn ngữ như PHP hay Java, để giải quyết vấn đề nhiều request cùng một lúc người ta sử dụng khái niệm đa luồng, tức là ứng với mỗi yêu cầu sẽ sinh ra một luồng mới để giải quyết. Nhưng vì node.js sử dụng đơn luồng tức là mọi request đều nằm trên một luồng chính (main thread). Nên các yêu cầu sẽ phải nằm trong Event Queue để đợi thực hiên, khi đó Event Loop sẽ tiến hành thực thi các tác vụ trên một luồng duy nhất theo cơ chế non-blocking I/O. Để giảm tải các tác vụ nặng nề, chúng sử dụng thêm Thread Pool.
- Thread Pool
    Thread pool cung cấp cho chúng ta 4 thread bổ sung hoàn toàn tách biệt với single thread trong Event Loop. Chúng ta có thể cấu hình nó lên đến 128 thread, nhưng thông thường, 4 thread này là đủ. Vì vậy, 4 thread này cùng nhau tạo thành một thread pool. Event loop sẽ tải bớt các tác vụ nặng vào Thread pool và điều này diễn ra tự động. Các DEV không phải là người quyết định cái gì đi vào Thread pool và cái gì không. Một số tác vụ tốn kém được giảm tải là : tất cả các hoạt động xử lý tệp, mọi thứ liên quan đến mật mã, như mật khẩu lưu vào bộ nhớ đệm, sau đó là tất cả các hoạt động liên quan đến nén, tra cứu DNS (khớp các miền web với địa chỉ IP thực tương ứng của chúng) và hơn thế nữa. Đó là những thứ dễ làm nghẽn luồng chính nhất. Vì vậy, Node sẽ xử lý nó bằng cách tự động tải chúng vào Thread pool.
    ![](https://i.imgur.com/KuNnJEr.png)

- Cấu trúc
![](https://i.imgur.com/bezSmdi.png)

# Module
- Module là các đoạn code được đóng gói lại với nhau và được giữ Private, chỉ các hàm và biến được định nghĩa bên trong Module là có thể truy cập và thao tác với nhau. Còn khi nào cần sử dụng Module từ bên ngoài thì chúng ta sẽ chìa các API là các biến, các hàm, hoặc cả 2 biến và hàm ra bên ngoài bằng cách sử dụng đối tượng exports hoặc module.exports.
- a module is a collection of JavaScript functions and objects that can be used by external applications

# Magic Number

![](https://i.imgur.com/ptFf2IC.png)



# Security Nodejs

- Giới hạn payload request gửi lên server
> app.use(express.json({limit: 300kB}))

# Debug
- **DEBUG'=*' node name_file**
- console.count: Hàm này dùng để đếm xem số lần object thực thi trong quá trình chạy.
- console.trace: cho biết đối tượng được tạo và thực hiện ở đâu
- console.memory: chương trình đg sử dụng Ram như thế nào
# Event loop
- Một process.nextTick cuộc gọi lại được thêm vào process.nextTick queue.
- Một Promise.then() cuộc gọi lại được thêm vào promises **microtask queue**. 
- A setTimeout, setImmediate gọi lại được thêm vào **macrotask queue**.
- setImmediate: đẩy lên đầu macrotask queue
- Even loop thực hiện process.nextTick queue trước tiên, sau đó thực hiện promises microtask queue và sau đó thực hiện macrotask queue



Dưới đây là một ví dụ để hiển thị thứ tự giữa và setImmediate():process.nextTick()Promise.then()

```javascript
const baz = () => console.log('baz')
const foo = () => console.log('foo')
const zoo = () => console.log('zoo')
const start = () => {
  console.log('start')
  setImmediate(baz)
  new Promise((resolve, reject) => {
    resolve('bar')
  }).then((resolve) => {
    console.log(resolve)
    process.nextTick(zoo)
  })
  process.nextTick(foo)
}
start()
```
// start foo bar zoo baz
Mã này trước tiên sẽ gọi start(), sau đó gọi foo() vào process.nextTick queue. Sau đó, nó sẽ xử lý promises microtask queue, in bar và thêm zoo() vào process.nextTick queue cùng một lúc. Sau đó, nó sẽ gọi zoo() cái vừa được thêm vào. Cuối cùng, baz() in macrotask queue được gọi.


![](https://i.imgur.com/EX1rXbR.png)

![](https://i.imgur.com/k3DUhI9.png)


- Node.js là xử lý đơn luồng, nên nó sẽ chỉ làm một việc một lúc. 
- Thực thi theo Queue
- Call stack là một LIFO (Last In, First Out) queue, event loop liên tục kiểm tra call stack để xem có func cần thực thi hay k, và thực hiện theo thứ tự => lặp lại cho đến khi call stack rỗng
- **Event Queue** là nơi mà các sự kiện do người dùng thực hiện như click, gõ phím hay fetch API sẽ được đẩy vào một hàng đợi và chờ đến khi được lấy ra và thực thi.
- **Event Loop** sẽ liên tục tìm kiếm trong call stack những gì cần thực thi cho đến khi call stack rỗng nhưng Event Loop sẽ không dừng ở đó, nó sẽ bắt đầu đọc tiếp Event Queue để nhặt ra những gì được nhét vào đó và lôi ra để thực thi.

- **Tips JS**

![image](https://hackmd.io/_uploads/H1hfICRaT.png)

- check trong macro task có microtask không, nếu có thì thực thi trước

![image](https://hackmd.io/_uploads/Sy0y8AAa6.png)
    
    - macro-task bao gồm các function trên 

# NodeJS And ExpressJs
- npm root -g: get location root global 
- Nodejs sử dụng Event-Driven Architecture (Kiến trúc theo hướng sự kiện)
- Nodejs nhanh khi công việc liên quan client tại bất kì time là nhỏ
- Nodejs sử dụng threads nhở để xử lý nhiều client, có hai type threads chính:
    - event loop(hay main loop, main thread, event thread, etc.
    - 1 pool of `k` Worker trong một Worker Pool (hay threadpool) (được triên khai trong `libuv`)



- router.all("*", [callback, ...] callback): 
```javascript=
router.all('*', requireAuthentication, loadUser)

- yêu cầu các router sau nó thực hiện requireAuthentication, loadUser
- Đầy không là **end point**
```



# Command
- -D ~ --save-dev
- console.time(): bắt đầu tính time
- console.timeEnd(): kết thúc tính thời gian
- npm root -g: check node_modules global
- engines: đặt version and other command mà package active

# Npm

![](https://i.imgur.com/6KWvPpk.png)
# package.json
- main: end point cho một packages, khi import vào một app thì là nơi tìm các module export
- private: true => xuất bản trên npm

# Lấy thông tin ra khỏi một path
- dirname: parent folder của một file
- basenam: lấy phần tên tệp
- extname: lấy phần mở rộng tệp

![](https://i.imgur.com/XoEUaex.png)

![](https://i.imgur.com/yhZaNUk.png)

# Node, accept arguments from the command line
- arg trong command line có:
    - arg1: full path `node` command line (path to node.exe)
    - arg2: full path file being executed
>const args = process.argv.slice(2) => get arg print

# Core and thread
![](https://i.imgur.com/KZZLone.png)

- Lõi (core) là một thành phần phần cứng vật lý trong khi luồng (thread) là thành phần ảo quản lý các tác vụ của lõi.
- Các lõi cho phép hoàn thành nhiều công việc hơn cùng một lúc, trong khi các luồng nâng cao tốc độ và thông lượng tính toán.
- Các lõi sử dụng chuyển đổi nội dung nhưng các luồng sử dụng nhiều bộ xử lý để thực hiện các quy trình khác nhau.
- Các lõi chỉ yêu cầu một đơn vị xử lý duy nhất; luồng yêu cầu nhiều đơn vị xử lý để thực hiện tác vụ.
- Các lõi tiêu thụ nhiều năng lượng hơn khi tải tăng lên trong khi các luồng có thể phối hợp với HĐH và các hạt nhân để chạy nhiều quy trình đồng thời một cách hiệu quả.


# Worker Thread

- Cluster trong Node.js là một module được sử dụng để tạo và quản lý một nhóm các quá trình con (worker processes) trong một ứng dụng Node.js. Nó cho phép ứng dụng tận dụng tối đa sức mạnh xử lý đa luồng của hệ thống để tăng hiệu suất và khả năng mở rộng.

- Khi một ứng dụng Node.js được chạy trong môi trường cluster, nó sẽ tạo ra một quá trình chính (master process) và một hoặc nhiều quá trình con (worker processes). Quá trình chính có nhiệm vụ tạo, quản lý và phân phối công việc cho các quá trình con. Các quá trình con được tạo ra để xử lý các yêu cầu và tác vụ trong ứng dụng.

- Các quá trình con trong cluster chạy độc lập và có thể chia sẻ cổng mạng để lắng nghe các yêu cầu từ client. Khi một yêu cầu đến, quá trình chính sẽ phân phối yêu cầu cho một quá trình con đang trống và có sẵn. Sau khi quá trình con hoàn thành xử lý yêu cầu, kết quả sẽ được trả về cho client thông qua quá trình chính.

- Một số ưu điểm của sử dụng Cluster trong Node.js là:

- Tăng hiệu suất: Bằng cách sử dụng nhiều quá trình con, ứng dụng có thể xử lý đồng thời nhiều yêu cầu và tác vụ, từ đó tăng hiệu suất của ứng dụng.

- Mở rộng khả năng xử lý: Cluster cho phép ứng dụng mở rộng khả năng xử lý bằng cách sử dụng tất cả các lõi và sức mạnh xử lý có sẵn trên hệ thống.

- Tăng tính ổn định: Nếu một quá trình con gặp lỗi hoặc sự cố, quá trình chính có thể tạo ra quá trình con mới để thay thế, đảm bảo tính ổn định của ứng dụng.

- Phân tán tải: Cluster cho phép phân tán tải công việc và yêu cầu giữa các quá trình con, giúp cân bằng tải và tránh quá tải cho một quá trình duy nhất.

- Để sử dụng Cluster trong Node.js, bạn có thể sử dụng module tích hợp sẵn "cluster". Nó cung cấp các API và sự kiện để tạo, quản lý và giao tiếp với các quá trình con.

- Tóm lại, Cluster trong Node.js cho phép tận dụng sức mạnh xử lý đa luồng của hệ thống để tăng hiệu suất và khả năng mở rộng của ứng dụng. Nó là một công cụ hữu ích khi bạn muốn xử lý đồng thời nhiều yêu cầu và tác vụ trong một ứng dụng Node.js.


- Worker threads can run CPU-intensive JavaScript operations without blocking the event loop from running. Unlike child_process, worker_threads can share memory by transferring ArrayBuffer instances or sharing SharedArrayBuffer instances.
- Workers are useful for performing CPU-intensive JavaScript operations; do not use them for I/O, since Node.js’s built-in mechanisms for performing operations asynchronously already treat it more efficiently than Worker threads can.
- Nói đến I/O ở đây người ta thường liên tưởng đến những công việc liên quan đến đọc/ghi dữ liệu vào file, hay các request http…
- Worker threads là một module trong node.js cho phép chạy mã Javascript song song với luồng chính. Mỗi worker được chạy độc lập với nhau, tuy nhiên chúng có thể giao tiếp với nhau thông qua postMessage()
- Xử lý những trường hợp tính toán dữ liệu lớn hoặc phức tạp để tránh việc chặn luồng chính
- Workers (threads) are useful for performing CPU-intensive JavaScript operations. They do not help much with I/O-intensive work. The Node.js built-in asynchronous I/O operations are more efficient than Workers can be.


# Cluster
- Chia sẻ cổng giữa các process sử dụng phương pháp lập lịch round robin (lập lịch xoay vòng, các process có cùng thời gian chạy)
- Clustering shines when it comes to CPU-intensive tasks.
- To split a single Node process into multiple processes
- The cluster module provides a way of creating child processes that runs simultaneously and share the same server port.
- Node.js runs single threaded programming, which is very memory efficient, but to take advantage of computers multi-core systems, the Cluster module allows you to easily create child processes that each runs on their own single thread, to handle the load.
- Các kết nối đến được phân phối giữa các child process theo một trong hai cách:
    - Master process lắng nghe các kết nối trên một cổng và phân phối chúng cho các worker theo kiểu vòng tròn. Đây là cách tiếp cận mặc định trên tất cả các nền tảng, ngoại trừ Windows.
    - Master process tạo một listen socket và gửi nó đến interested workers, sau đó các workers có thể trực tiếp chấp nhận các kết nối đến.
- As long as there are some workers still alive, the server will continue to accept connections. If no workers are alive, existing connections will be dropped and new connections will be refused.
- Mô-đun cluster Node.js cho phép tạo các child process (worker) chạy đồng thời và chia sẻ cùng một cổng máy chủ. Mỗi child process được sinh ra có even loop, bộ nhớ và phiên bản V8 riêng. The child processes use IPC (Inter-process communication) để kết nối với parent process
-  The worker processes are spawned using the child_process.fork() method. The method returns a ChildProcess object that has a built-in communication channel that allows messages to be passed back and forth between the child and its parent. It is recommended to not create more workers than there are logical cores on the computer as this can cause an overhead in terms of scheduling costs. This happens because the system will have to schedule all the created processes so that each gets a turn on the few cores.
-  The workers are created and managed by the master process. When the app first runs, we check to see if it's a master process with isMaster. This is determined by the process.env.NODE_UNIQUE_ID variable. If process.env.NODE_UNIQUE_ID is undefined, then isMaster will be true.
>var cluster = require('cluster');

# Child process

- child_process.spawn(): Phương thức này cho phép bạn chạy một lệnh hệ thống trong một quá trình con. Nó trả về một đối tượng ChildProcess, cho phép bạn gửi và nhận dữ liệu qua luồng nhập/ xuất tiêu chuẩn.

- child_process.exec(): Phương thức này cho phép bạn chạy một lệnh hệ thống trong một quá trình con, tương tự như child_process.spawn(). Tuy nhiên, child_process.exec() sử dụng một shell để thực thi lệnh, cho phép bạn sử dụng các biểu thức chính quy và các tính năng của shell.

- child_process.fork(): Phương thức này được sử dụng để tạo ra một quá trình con mới chạy một module Node.js. Quá trình con này có thể gửi và nhận dữ liệu qua đối tượng send() và sử dụng process.on('message') để lắng nghe các tin nhắn từ quá trình cha.

- The child_process module provides the ability to spawn new processes which has their own memory. The communication between these processes is established through IPC (inter-process communication) provided by the operating system.
-  used by exec() and execFile(), all the processed data is stored in the computer’s memory. 
-  The spawn() function runs a command in a process. This function returns data via the stream API. Therefore, to get the output of the child process, we need to listen for stream events. => you can process a large amount of data without using too much memory at any one time.
-  fork() enables communication between the parent and the child process.
    -  in addition to retrieving data from the child process, a parent process can send messages to the running child process. Likewise, the child process can send messages to the parent process.

# Stream

- Node.js streams provides four types of streams:
    - Readable Streams
    - Writable Streams
    - Duplex Streams
    - Transform Streams

- `readableFlowing`: xác định xem có flow data? (boolean)
- `pause()`: tạm dừng flow stream
- `consume()`: tiếp tục flow stream
## Readable Stream
- `process.stdin` - To read user input via stdin in a terminal application.
- `http.IncomingMessage`- To read an incoming request's content in an HTTP server or to read the server HTTP response in an HTTP client.
## Writable Stream
- `process.stdout` can be used to write data to standard output and is used internally by console.log.
## Duplex Streams
- Read and Write
- readable and writable sides operate independently from one another in a duplex stream. The data does not flow from one side to the other.
## Transform Streams
- the readable side is connected to the writable side in a transform stream
# Buffer && Stream
- buffer.constants.MAX_LENGTH: check size max của buffer
- Buffer and Stream in Node.js are actually similar to the general concepts
- A buffer is a temporary memory that a stream takes to hold some data until it is consumed.

- In a stream, the buffer size is decided by the highWatermark property on the stream instance which is a number denoting the size of the buffer in bytes
-  buffer is a space in computer memory, usually RAM, that stores binaries

>When reading a file, Node.js allocates memory as much as the size of a file and saves the file data into the memory
Buffer indicates the memory where file data resides

- Problem of Buffer
readFile() buffer method is convenient but has a problem that you need to create 100MB buffer in a memory to read 100MB file
If you read 10 100MB files, then you allocate 1GB memory just to read 10 files
Especially, it becomes a big problem for a server, given that you do not know how many people are going to use (read file) concurrently

## Pipe

- Piping (đường ống) cho phép chúng ta lấy dữ liệu đầu ra từ một stream làm đầu vào trong streams khác. Nó hoạt động như một đường ống giúp chuyển dữ liệu giữa các streams với nhau.

![](https://i.imgur.com/pTpaevE.png)

- So sánh child process và worker threads

![](https://hackmd.io/_uploads/H1T33ExWp.png)

# Process And Thread
![](https://i.imgur.com/6GcVvaj.png)

- khi chạy một program bằng node command thì tạo một process, hệ điều hành sẽ cấp phát bộ nhớ cho chương trình và xác định chương trình thực thi trên máy tính và tải chương trình vào bộ nhớ
- Process: là một chương trình chạy trong hệ điều hành, có bộ nhớ riêng, chỉ có một tasks trong một thời điểm
- Thread: giống process, có con trỏ lệnh riêng và có thể thực thi một tác vụ JavaScript tại một thời điểm. Không giống như các process, các thread không có bộ nhớ riêng mà chúng nằm trg bộ nhớ của process
    - A thread is a virtual component that handles the tasks of a CPU core, to complete them in an effective manner. It is a unit of execution on simultaneous programming.
- Khi tạo một process có thể có nhiều thread, thread có thể giao tiếp với nhau qua truyền tin nhắn hoặc chia sẻ data bộ nhớ của process => nhẹ hơn so với process do không sử dụng bộ nhớ
- Node.js triển khai thư viện libuv, cung cấp thêm bốn luồng cho process Node.js, ngoài ra còn V8 engine cung cấp 2 thread để tự động thu gom rác  => 7 thread: 1 main thread, 4 thread nodejs, 2 thread V8
- `sudo kill -9 'pgrep node'`: kill all process
- `top -H -p 9933`


- CPU-Bound: khi thời gian để nó hoàn thành một tác vụ được xác định chủ yếu bởi tốc độ của bộ xử lý trung tâm (central processor)

## Multithreading Nodejs

- Nodejs runs Js code in a single thread: code run can one tash at a time
- Nodejs itsefl is multithreading and provides hidden threads through library `lib`, which handles I/O operations like reading files from disk or netword request => use hidden thread allows code  make I/O request without blocking main thread

- Cannot use hidden threads to offload CPU - intensive tasks (k sử dụng làm giảm tác vụ giảm CPU như tính toán phức tạp, thay đổi kích thước ảnh, nén video)
- Js is single-threaded when a CPU - intensive tasks, its block main thread and code others not executes until the task completes. Without using other threads, the only way to speed up a CPU-bound task is to increase the processor speed.

- node name_file arg1 &
    - &: cho phép chạy trong nền
- khi chạy một chương trình bằng command node => tạo một process, hệ điều hành phân bổ bộ nhớ cho program (chương trình) => định vị program => load program vào bộ nhớ => assign process ID => executing the program => become process


##

- Npm
    - Gulp: header, footer giống nhau ở nhiều trang thì sử dụng để k p viết lại code
- Sử dụng `require('name_module')`
- Sử dụng `createServer()` để tạo một HTTP Server
    + createServer(function(req, res)):
        + req:  là yêu cầu từ client dưới dạng một Obj
- res.writeHead(param1, param2) 
    - param1: là status code
    - param2: là content
- Làm việc với File Server: require('fs')
    - name_val.readFile(): read file on your computer
    - Tạo file: fs.appendFile()
    - fs.open()
    - fs.writeFile()
-`Get` method: nhận dữ liệu từ server về client, nhận dữ liệu từ máy ch
ủ về phía client (gắn query method)
- `Post` method: nhận dữ liệu từ client tới sever (không gắn method query)

# Express

## Application-level middleware
- app.use(path, middleware)
-  Khi muốn bỏ qua các hàm middleware tiếp theo không thực hiện nữa, chúng ta sẽ sử dụng lệnh next('route'), tuy nhiên việc này chỉ tác dụng với các hàm middleware được load thông qua hàm app.METHOD hoặc router.METHOD.
## Router-level middleware

- router.use(middleware): thực hiện mỗi lần req vì k có path cụ thể
## Error-handling middleware

```javascript=
app.use(function (err, req, res, next) {
  console.error(err.stack)
  res.status(500).send('Something broke!')
})

```
### Handle Error

- Trong TH Đồng bộ
```javascript=
app.get('/', (req, res) => {
  throw new Error("Hello error!")
})

<=>
    
app.get('/', (req, res, next) => {
  try {
      throw new Error("Hello error!")
  }
  catch (error) {
      next(error)
  }
})
```
- Khi một lỗi gặp phải trên code đồng bộ, Express sẽ tự động bắt nó

- Trong TH k đồng bộ thì k áp dụng được ta sử dụng hàm next(erro) hoặc sử dụng **error-async-middlware** (npm)

- Example hanle error by middleware
```javascript=
const express = require('express')
const fsPromises = require('fs').promises

const app = express()
const port = 3000

app.get('/one', (req, res, next) => {
  fsPromises.readFile('./one.txt') // arbitrary file
    .then(data => res.send(data))
    .catch(err => next(err)) // passing error to custom middleware
})

app.get('/two', (req, res, next) => {
  fsPromises.readFile('./two.txt')
    .then(data => res.send(data))
    .catch(err => {
        err.type = 'redirect' // custom prop to specify handling behaviour
        next(err)
    })
})

app.get('/error', (req, res) => {
  res.send("Custom error landing page.")
})

app.use((error, req, res, next) => {
  console.log("Error Handling Middleware called")
  console.log('Path: ', req.path)
  console.error('Error: ', error)
 
  if (error.type == 'redirect')
      res.redirect('/error')

   else if (error.type == 'time-out') // arbitrary condition check
      res.status(408).send(error)
  else
      res.status(500).send(error)
})


app.listen(port, () => {
  console.log(`Example app listening at http://localhost:${port}`)
})

```
```javascript=
const express = require('express')
const fsPromises = require('fs').promises

const app = express()
const port = 3000

app.get('/one', (req, res, next) => {
  fsPromises.readFile('./one.txt')
  .then(data => res.send(data))
  .catch(err => next(err)) // passing error to custom middleware
})

app.get('/two', (req, res, next) => {
  fsPromises.readFile('./two.txt')
  .then(data => res.send(data))
  .catch(err => {
      err.type = 'redirect' // adding custom property to specify handling behaviour
      next(err)
  })
})

app.get('/error', (req, res) => {
  res.send("Custom error landing page.")
})

function errorLogger(error, req, res, next) { // for logging errors
  console.error(error) // or using any fancy logging library
  next(error) // forward to next middleware
}

function errorResponder(error, req, res, next) { // responding to client
  if (error.type == 'redirect')
      res.redirect('/error')
  else if (error.type == 'time-out') // arbitrary condition check
      res.status(408).send(error)
  else
      next(error) // forwarding exceptional case to fail-safe middleware
}

function failSafeHandler(error, req, res, next) { // generic handler
  res.status(500).send(error)
}

app.use(errorLogger)
app.use(errorResponder)
app.use(failSafeHandler)

app.listen(port, () => {
console.log(`Example app listening at http://localhost:${port}`)
})

```

## Error handling in Express
- Sử dụng error.track để print các function gọi trước khi bị lỗi

![](https://hackmd.io/_uploads/Sk_k6egWp.png)

![](https://hackmd.io/_uploads/SJqNpxlZp.png)


# Version
- npm list -g: get all npm local
- version thường có 3 số (e.g: 1.1.0): 
    - first: bản chính
    - second: bản phụ
    - third: bản vá
- ~0.13.0: cập nhập bản vá lỗi
- ^1.13.0: cập nhập bản bản phụ và vá lỗi
- 1.13.0: giữ nguyên
- Package-lock.json đặt phiên bản hiện đã được cài đặt của bạn của mỗi package => để giữ nguyên k cập nhập bản vá, bản phụ mới sử dụng `npm ci`
- `npm update` cập nhập package-lock
- tìm các package npm đã cài đặt: npm list
- cài đặt phiên bản bất kỳ `npm install <package>@<version>`
- check new version in npm: `npm view [package_name] version` 
- npm express-validator: xác thực
- npm formidable: xử lý đầu vào của form
![](https://i.imgur.com/WSjyFlz.png)

