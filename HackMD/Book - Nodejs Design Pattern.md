---
title: Book - Nodejs Design Pattern

---

# Nodejs Design Pattern

## ARCHITECT
- **Node.js** là một **môi trường** chạy mã Javascript, không p là ngôn ngữ lập trình vì nó cung cấp môi trường thông qua V8 Engine của Chrome
- JS là singe thread ( nếu có 2 luồng tại cùng một thời điểm để brower lắng nghe, ví dụ theo tác DOM sửa xóa thì nó k biết được p thực hiện hành động nào => p là đơn luồng)

![](https://hackmd.io/_uploads/SJTS5DQMa.png)

:::info
- **V8**: Đây là bộ Engine V8 của chrome, bao gồm Memory Heap, Call Stack, Garbage Collector và chuyển đổi mã Javscript thành mã máy của hệ điều hành. (nơi thực thi JS)
- **Libuv**: gồm Thread Pool, Event Loop, là thư viện đa nền tảng tập trung sử dụng các tác vụ I/O không đồng bộ
- **Node.js Standard Library**: liên quan đến library timer, file system, network call
:::

---

- **Tổng quát về các thành phần có trong NodeJSs**
<br/>
![](https://hackmd.io/_uploads/SkzI0PQMp.png)

:::info
**Call Stack**: code JS được đưa vào để xử lý, tại một thời điểm chỉ có một đoạn code được xử lý ( là nơi thực thi JS)
```javascript=
const add = (a, b) => a + b;
const multiply = (a, b) => a * b;

const addCofficient = (val) => multiply(val, 1.8);
const addConst = (val) => add(val, 32);

const convertCtoF = (val) => {
  let result = val;
  result = addCofficient(result);
  result = addConst(result);
  return result;
};

convertCtoF(100);

=> run theo depth 
(convertCtoF -> addCofficient -> multiply -> ...)
```
:::

:::info
- ***LibUv***:
    - **Event Queue (Message Queue)** : 
        - lưu trữ các event, callback, ...
        - khi một tác vụ được hoàn thành nó sẽ được đưa vào Event Queue để xử lý
    - **Thread Pool**: xử lý blocking I/O - **external resources**(database, network, file systerm, ... ) => sau khi hoàn thành đưa vào Event Queue
        - File systerm
        - DNS
        - Code user setting
   - **Event Loop**: liên tục kiểm tra xem Call Stack còn trống không? Nếu có nó sẽ đưa tất cả event/callback từ Event Queue tới Call Stack để thực hiện
:::



:::info
- Event Queue
<br/>

![](https://hackmd.io/_uploads/r1ezJi4Ga.png)
```javascript=
while (true) {
    if (the call stack is empty) {
        while (the microtask queue is not empty) {
            dequeue the oldest (micro) task and run it (push it to the call stack)
        }
    }
    
    if (the macrotask queue is not empty) {
        dequeue the oldest (macro) task and run it (push in to the call stack)
    }
    
    repaint()
}
```

:::
[[Demo]]

:::warning
JS là đơn luồng, NodeJS có thể gọi là đa luồng vì nó thực hiện các tác vụ tốn time ở Libuv theo phong cách multithreading
Code JS được xử lý bằng 1 luồng duy nhất chính là V8 engine(main thread), còn các thứ chạy bên dưới bởi libuv thì đa luồng(worker thread).

- Code sẽ chạy từ trên xuống tuần tự, khi gặp bất đồng bộ nó sẽ đẩy sang node api để xử lý (callback) => push vào event queue (macro hoặc micro)
:::
- **Tác vụ I/O**: liên quan đến read/write file và liên quan đến network
    - Các tác vụ I/O tốn thời gian được đưa vào Thread Pool ( readFile, writeFile, http.get, ...) để xử lý


### NodeJS xử lý bất đồng bộ như thế nào
- Event Loop luôn kiểm tra trong call stack có trống không và nếu trống nó sẽ chuyển các hàm callbacks từ Event queue vào call stack lần lượt theo thứ tự FIFO (First In First Out).





## Platform
- Node.js core: Nodejs runtime, built-in modules
- modules: structuring the code of a programs
- How to work
- I/O is slow
- Block I/O
    - to handle many connection when block thread, i can use separate thread (or process) but thread is not cheap in terms of system resource (consume memory and context switches - so having a long-running thread for each connection and not using it for most of the time means wasting precious memory and CPU cycles)
- Non-blocking I/O:
    - return immediately without waiting for the data to be read or writen. If no results are available at the moment of the call, the function will simply return a predefined constant, indicating that there is no data available to return at that momen
- Event demultiplexing (synchronous event demultiplexer (also known as the event notification interface))
### Reator Pattern
![](https://hackmd.io/_uploads/rksWXXzMa.png)

- Even demultiplexer: chỉ định handler (được gọi khi hoạt động hoàn thành) -( non-blocking)
- Even demultiplexer hoản thành, thêm một Event vào **Event Queue**
- **Event loop** sẽ lặp qua từng event queue và gọi handler tương ứng

![](https://hackmd.io/_uploads/HyjhcXzfT.png)

### Libuv, the I/O engine of Node.js
- Mỗi OS có một Event Demultiplexer riêng, nên sử dụng libuv tương thích với nhiều hệ điều hành trên non-blocking 
### The recipe Nodejs

![[https://hackmd.io/_uploads/B1YHOpzGT.png]]
[https://hackmd.io/_uploads/B1YHOpzGT.png]()
![[Pasted image 20240624231923.png]]