---
title: Operating System
tags: [School]

---

###### tags: `School`
# Operating System

- OS: một phần mềm hoạt động như một giao diện giữa các thành phần phần cứng máy tính và người dùng, một hệ thống máy tính cần ít nhất một máy tính
![](https://i.imgur.com/GW0e5BQ.png)

## Các loại hệ điều hành
### Batch OS: 
    - To speed the same process, a job with a similar type of needs are batched together and run as a group.
    - User k tg tác trực tiếp với máy tính
    - các chương trình được đưa và hàng chờ, và máy tính chạy tuần tự
### Multi-Tasking/Time-sharing Operating systems: 

- Time-Sharing Operating System là một loại hệ điều hành trong đó người dùng có thể thực hiện nhiều tác vụ và mỗi tác vụ có cùng một khoảng thời gian để thực hiện
- Giảm thiểu thời gian phản hồi của CPU

### Real Time
- khoảng thời gian để thực hiện và phản hồi đầu vào là nhỏ
### Distributed Operating System
- Sử dụng nhiều bộ xử lý nằm ở máy khác => tăng tốc độ tính toán
### Network Operating System
- nằm trên server, cung cấp khả năng phục vụ để quản lý dữ liệu, người dùng, nhóm, bảo mật, ứng dụng và các chức năng mạng khác.

## Function of OS
![](https://i.imgur.com/QhAv46z.png)


- **Process management**: tạo và xóa process, cung cấp cơ chế đồng bộ và không đồng bộ, giao tiếp giữa các process
- **Memory management**: cập phát và hủy
- **Command interpretation**
- **Job accounting**: cấp phát bộ nhớ cho các chương trình cần nhiều tài nguyên
- **File management**:
- **Device Management**:
- **I/O System Management**:
- **Secondary-Storage Management**
- **Security**:
- **Networking**
- **Command interpretation**
- **Job accounting**
- **Communication management**: phối hợp và phân công compiler (biên dịch), interpreters (thông dịch) và một số tài nguyên của người dùng khác nhau của hệ thống máy tính

## Kernel
- là hạt nhân của máy tính, thành phần trung tâm của hệ điều hành máy tính
- công việc duy nhất: quản lý giao tiếp giữa phần mềm và phần cứng
- là một trong những chương trình đầu tiên được tải sau khi khởi động bootloader
- cung cấp quyền truy cập an toàn vào phần cứng cho các chương trình khác nhau
![](https://i.imgur.com/hH5onD4.png)

### Features of Kernel 
- Low-level scheduling of processes
- Inter-process communication
- Process synchronization
- Context switching

### Type of Kernel
- Có loại phổ biến: Monolithic và Microkernels
    - **Monolithic**: một khối duy nhất của chương trình, chạy ở một địa chỉ duy nhất
    - **Microkernels** (vi nhân): 
        - quản lý tất cả các tài nguyên của hệ thống
        - service triển khai ở các địa chỉ khác nhau
        - các dịch vụ người dùng được lưu trữ trong không gian địa chỉ người dùng và các dịch vụ hạt nhân được lưu trữ trong không gian địa chỉ hạt nhân: giúp giảm kích thước kernel và hệ điều hành

## Microkernel in Operating System: Architecture, Advantages

- **Microkernel**: là một phần mềm hoặc code chứa tối thiểu func, data và thuộc tính để triển khai hệ điều hành, thường được triển khai bằng C, C++ và một chút hợp ngữ
- **Architecture Microkernel**:
![](https://i.imgur.com/MpjG0pc.png)
- Các thành phần của microkernel
    - gồm các chức năng cốt lõi của hệ thống, nếu đặt chúng ở bên ngoài sẽ làm gián đoạn chức năng của hệ thống
### So sánh monolithic và microkernel



![](https://i.imgur.com/h9bK86Q.png)

## Các thành phần của hệ điều hành

![](https://i.imgur.com/ZFgZ5js.png)
- File Management: một file là tập hợp thông tin được xác định bởi người tạo nó
    - chức năng: 
        - Tạo và xóa tệp và thư mục.
		- Để thao tác các tập tin và thư mục.
		- Ánh xạ các tập tin vào bộ nhớ thứ cấp.
		- Sao lưu tập tin trên phương tiện lưu trữ ổn định.
- Process Management: việc thực hiện process p tuần tự
    - chức năng:
		- Quá trình tạo và xóa.
		- Đình chỉ và tiếp tục.
		- Quá trình đồng bộ hóa
		- Quá trình giao tiếp

- I/O Device Management: ẩn các biến thể cụ thể của phần cứng khỏi ng dùng
    - chức năng
		- Nó cung cấp hệ thống bộ nhớ đệm
		- Nó cung cấp mã trình điều khiển thiết bị chung
		- Nó cung cấp trình điều khiển cho các thiết bị phần cứng cụ thể.
		- I/O giúp bạn biết các đặc điểm riêng của một thiết bị cụ thể.
- Network Management: Nó bao gồm quản lý hiệu suất, phân tích lỗi, cung cấp mạng và duy trì chất lượng dịch vụ.
    - chức năng: 
    - ng

- Secondary-Storage Management: nhiệm vụ quan trọng nhất của hệ thống máy tính, là thực thi các chương trình., Các chương trình như trình hợp dịch, trình biên dịch, được lưu trữ trên disk cho đến khi nó được tải vào bộ nhớ, sau đó sử dụng disk làm nguồn và đích để xử lý
    - chức năng: 
        - storage allocate
        - free space management
        - disk schedule

- Security Management: quy trình khác nhau trong một hệ điều hành cần được bảo mật khỏi các hoạt động của nhau.
    - Các hoạt động khác của:
        - người dùng k thể I/O trực tiếp

- Memory Main Management: tiến hành bằng cách sử dụng một chuỗi các lần đọc hoặc ghi các địa chỉ bộ nhớ nhất định.

## Một số cấu trúc của hệ điều hành
- Đơn giản (Simple approach): 
- Phân tầng (Layered apparoach)
- Vi nhân (Microkernel)
    - giữ cho nhân có các đủ các chức năng thiết yếu để giảm cỡ
    - các chức năng khác được đưa ra ngoài nhân
- Module (Module approach)

## System Call
- Cơ chế cung cấp giao diện giữa process và OS(operating system)
- computer program requests a service from the kernel of the OS
- System call offers the services of the operating system to the user programs via API (Application Programming Interface). System calls are the only entry points for the kernel system.
### How System Call Works?
![](https://i.imgur.com/C66ezWo.png)

b1: processes thực thi ở chế độ người dùng cho đến khi system call làm gián đoạn nó
b2: system call thực thi nó ở kernel mode với chế độ ưu tiên
b3: sau khi system call thực thi xong thì quay lại chế độ người dùng
b4: việc thực thi các quy trình người dùng được tiếp tục ở chế độ kernel

### Why do you need System Calls in OS?
![](https://i.imgur.com/ywVQeUU.png)

### Types of System calls
![](https://i.imgur.com/wmJVTYf.png)

### Rules for passing Parameters for System Call
- Parameters should be pushed on or popped off the stack by the operating system.
- Parameters can be passed in registers.
- When there are more parameters than registers, it should be stored in a block, and the block address should be passed as a parameter to a register.

### Important System Calls Used in OS

![](https://i.imgur.com/pdWPXx8.png)



## Processing
- là một chương trình đang được thực thi trong hệ thống máy tính. Mỗi tiến trình có thể thực hiện một hoặc nhiều nhiệm vụ, và có thể sử dụng các tài nguyên của hệ thống như bộ nhớ, bộ vi xử lý, các tệp tin, các thiết bị I/O, và các tiến trình khác.
- gồm: 
    - code, 
    - đoạn dữ liệu, 
    - đoạn ngăn xếp và heap(stack/heap)
    - các *hoạt động hiện tại* được thể hiện qua con đếm lệnh (IP) và nội dung các thanh ghi (registers) của bộ xử lý
- chú ý:
    - tiến trình là thực thể chủ động
    - chương trình là thực thể bị động

### Trạng thái tiến trình

![](https://i.imgur.com/WbrpZGv.png)

### Khối điều khiển tiến trình

![](https://i.imgur.com/7PKy6QJ.png)


## Process Scheduling


### Hàng chờ lập lịch process
![](https://i.imgur.com/IBowEVx.png)

- các tiến trình chưa được phân phối sử dụng CPU sử được đưa vào hàng chờ (queue)
- có thể có nhiều hàng chờ trong hệ thống
- running => ready: hết thời gian làm việc
- waiting => ready: vào ra xg ngắt xg



- FCFS (First Come First Serverd): 
- SJF (Shortest Job First): thời gian còn lại để thực hiện hết process đó


=> thời gian thực => tương tác => xử lý theo lô
- 1 epoch: khi 
- 
- các process đang chạy, chạy hết 1 vòng 

### Phân loại các bộ lập lịch

- bộ lập lịch dài hạn 
    - thường sử dụng trong các hệ xử lý theo lô
    - đưa tiến trình từ spool vào bộ nhớ trong
- bộ lập lịch ngắn
    - còn gọi là bộ lập lịch CPU
    - lựa chọn tiến trình tiếp theo sử dụng CPU
- bộ lập lịch trung hạn
    - còn gọi là swapping (tráo đổi)
    - di chuyển tiến trình đang trong trạng thái chờ giữa bộ nhớ trong và bộ ngoài

### Bộ lập lịch trung hạn

![](https://i.imgur.com/HHVSSrU.png)

### Hàng chờ lập lịch tiến trình 

![](https://i.imgur.com/o7Gz5xj.png)

### Chuyển trạng thái (Context switch)

- Xảy ra khi process A bị ngắt khỏi CPU, B bắt đầu sử dụng CPU
- Cách thực hiện:
    - Nhân hệ điều hành ghi lại toàn bộ status A, lấy từ PCB (khối điều khiển tiến trình) của A
    - Đưa A vào hàng chờ 
    - Nhân hệ điều hành nạp trạng thái của B lấy từ PCB của B
    - Thực hiện B
=> lãng phí thời gian ( 1-1000 micro giây) => càng nhanh càng tốt


## Các thao tác với process
### Tạo process
- func `create-process` tạo process mới
    - process gọi create-process là tiến trình cha
    - tiến trình được tạo gọi là tiến trình con
- parent process có thể thực hiện song song hoặc chờ child process kết thúc
- gọi/không gọi `wait` để chờ/không chờ tiến trình con kết thúc
### Kết thúc tiến trình
- kết thúc bình thường: exit
- kêt thúc bất thường (lỗi hoặc có sự kiện): abort hoặc kill
    - sử dụng quá quota tài nguyên
    - child process không cần thiết
    - parent process đã kết thúc

### Hợp tác giữa các tiến trình
- khi
    - sử dụng chung tài nguyên
    - sử dụng một số nhiệm vụ chung
    - tăng tốc độ tính toán
- Để hợp tác các tiến trình, cần có các cơ chế truyền thông/liên lạc giữa các tiến trình (Interprocess communication – IPC)

## Truyền thống giữa các tiến trình (IPC)

### Các hệ thống truyền thông
- sử dụng toán tử: `receive` và `send`
- process cần có tên
- cần có 1 kết nối (login) giữa tiến trình P và Q để truyền thông điệp
- một số loại truyền thông 
    - trực tiếp và gián tiếp:
    - đối xứng và không đối xứng
    - sử dụng vùng đệm tự động hoặc không tự động 
#### Truyền thông trực tiếp

- toán tử
    - send (P, msg): gửi msg đến process P
    - receive(Q, msg): nhận msg từ process Q (cải tiến: nhận từ bất kỳ process nào)

- minh hoạ:
    - một cặp tiến trình chỉ có một kết nối duy nhất (kết nối cho cặp tiến trình duy nhất)
    - process cần biết tên/số hiệu của process kia là truyền thông được

![](https://i.imgur.com/ZlKZkkn.png)

#### Truyền thống gián tiếp
- thông điệp được nhận qua mail hoặc port
- toán tử: 
    - send (A, msg): gửi msg đến hộp thư A
    - receive (B, msg): nhận msg từ hộp thư B
- minh hoạ
    - 2 process có kểt nối nếu chung hộp thư
    - một kết nối có thử sử dụng nhiều tiến trình
    - nhiều kết nối có thể tồn tại giữa một cặp tiến trình ( sử dụng hòm thử khác nhau)

![](https://i.imgur.com/SvtrCOn.png)


### Vấn đề đồng bộ hoá (Synchronization)
- blocking: có chờ
- non-blocking: không chờ

![](https://i.imgur.com/iA6aVbD.png)

### Vấn đề sừ dụng bộ đệm (buffer)

- Các thông điệp nằm trong hành chờ tạm thời
- Cỡ của hàng chờ:
    - chứa được 0 thông điệp: send blocking
    - chứa được n thông điệp: send non-blocking cho đến khi hàng chờ có n thông điệp, sau đó send blocking
    - vô hạn: send non-blocking


## CPU Schelude 

- Tại một thời điểm chỉ có một process được thực hiện trên CPU
- Vấn đề:
    - số lượng yêu cầu sử dụng nhiều hơn số lượng tài nguyên đang có (CPU)
    - cần lập lịch để phân phối thời gian sử dụng CPU cho các tiến trìnủa người sử dụng và hệ thống

### CPU-burst và IO-burst
-  **CPU-burst** là **thời gian mà một tiến trình hoặc luồng sử dụng bộ vi xử lý** (CPU) để thực thi một tác vụ nhất định trước khi bị chuyển sang tiến trình hoặc luồng khác. (đang ở trạng thái running)
-  **IO-burst** là thời gian mà một tiến trình trong hệ thống máy tính sử dụng để truy xuất và thực hiện các hoạt động vào/ra (I/O), chẳng hạn như đọc hoặc ghi dữ liệu từ hoặc đến ổ cứng, thiết bị USB, mạng, hoặc các thiết bị I/O khác

### Hai loại tiến trình chính
- CPU-bound: thuật ngữ chỉ một tiến trình trong hệ thống máy tính mà **yêu cầu một lượng lớn thời gian xử lý của bộ xử lý trung tâm (CPU)** để thực hiện tác vụ của nó. Một tiến trình CPU-bound thường không tương tác với các thiết bị I/O, nghĩa là nó không chịu tác động đáng kể từ việc truy xuất dữ liệu từ đĩa cứng, USB hay thiết bị mạng. Thay vào đó, nó tập trung vào việc thực hiện tính toán hoặc xử lý dữ liệu.
- I/O-bound: chỉ các tiến trình trong hệ thống máy tính tốn nhiều thời gian để truy cập vào hoặc ghi dữ liệu từ các thiết bị vào/ra. Các tiến trình I/O-bound thường tập trung vào các hoạt động đọc/ghi dữ liệu, chẳng hạn như đọc/ghi tập tin trên ổ đĩa cứng, truy cập cơ sở dữ liệu từ xa qua mạng, đọc/ghi trên thiết bị USB, tải các tài nguyên từ internet, và nhiều hoạt động khác liên quan đến đọc/ghi dữ liệu.

### Bộ lập lịch ra hoạt động khi
1. process chuyển trạng thái từ running sang waiting
2. running => ready
3. waiting => ready
4. process kết thúc

### Các phương pháp lập lịch

![](https://i.imgur.com/lIJV8lt.png)

### Lập lịch non-preemptive
- 1 process giữ CPU đến khi nó kết thúc hoặc chuyển sang trạng thái waiting
- có thể sử dụng nhiều loại phần cứng vì không đòi hỏi timer

### Lập lịch preeprive
- hiệu quả hơn lập lịch non-preeptive
- thuật toán phức tạp hơn và sử dụng nhiều tài nguyên CPU hơn

### Bộ điều phối (dispatcher)
- nhiệm vụ
    - chuyển trạng thái
    - chuyển về user-mode
    - thực hiện tiến trình theo trạng thái đã lưu
- cần hoạt động hiệu quả (tốc độ nhanh)
- độ trễ (latency) của bộ điều phối: thời gian cần để điều phối dừng một process và thực thi process khác

### Các tiêu chí đánh giá lập lịch
- khả năng tận dụng CPU (CPU utilization)
    - thể hiện tải CPU - là một số từ 0 - 90% (40 tải thấp - 90 tải cao)
- thông lượng (throughput): Là số lượng các tiến trình hoàn thành trong một đơn vị thời gian
- thời gian hoàn thành (turnaround time):
    - t<sub>turnaround</sub> = to - t i với t i là thời điểm tiến trình vào hệ thống, to là thời điểm tiến trình ra khỏi hệ thống (kết thúc thực hiện)
    - Như vậy t turnaround là tổng: thời gian tải vào bộ nhớ, thời gian thực hiện, thời gian vào ra, thời gian nằm trong hàng chờ…
- thời gian chờ (waiting time): là tổng thời gian tiến trình phải nằm trong hàng chờ ready (t<sub>waiting</sub>)
    - các thuật toán lập lịch: k ảnh hưởng tổng thời gian thực hiện 1 process mà chỉ ảnh hưởng đến thời gian chờ của một tiến trình trg hàng chờ ready
- thời gian đáp ứng (response time): là khoảng thời gian từ khi tiến trình nhận được một yêu cầu cho đến khi bắt đầu đáp ứng yêu cầu đó
    - t<sub>res</sub> = t<sub>r</sub> - t<sub>s</sub>, trong đó t<sub>r</sub> là thời gian nhận yêu cầu, t<sub>thời điểm bắt đầu đáp ứng yêu cầu</sub>

### Đánh giá các thuật toán lập lịch

![](https://i.imgur.com/o6RjmSh.png)

### FCFS (First Come First Served)

- dễ gây ra hiện tượng đoàn hộ tống (convoy effect): process "lớn" nằm đầu hàng chờ và theo sau nó là nhiều các process con
 
### SJF (shortest job first)

- t<sub>nextburst</sub> nhỏ nhất chạy trước
- thường áp dụng cho lập lịch dài hạn

### Round-robin (RR)
- lập lịch xoay vòng: mỗi process có cùng một thời gian chay

## Đồng bộ hoá tiến trình

### Tương tranh và đồng bộ
- tình huống tương tranh (race condition): xuất hiện khi nhiều process cùng thao tác trên dữ liệu chung và kết quả các thao tác đó phục thuộc vào thứ tự thực hiện của các tiến tình trên dữ liệu chung
- để tránh các tình huống tương tranh, các tiến trình cần được đồng bộ theo một phương thức nào => vấn đề nghiên cứu: đồng bộ hoá các tiến trình
### Đoạn mã găng ( critical section )

- xét một hệ có n tiến trình P0 , P1 , ..., Pn , mỗi tiến trình có một đoạn mã lệnh gọi là đoạn mã găng, ký hiệu là CSi , nếu như trong đoạn mã này, các tiến trình thao tác trên các biến chung, đọc ghi file... (tổng quát: thao tác trên dữ liệu chung)

- hệ n tiến trình cần có: khi một tiến trình P<sub>i</sub> thực hiện đoạn mã CS<sub>i</sub> thì không có tiến trình P<sub>j</sub> nào khác được phép thực hiện CS<sub>j</sub>
- mỗi tiến trình P<sub>i</sub> phải "xin phép" (entry section) trước khi thực hiện CS<sub>i</sub> và thông báo (exit section) cho các tiến trình khác sau khi thực hiện xong CS<sub>i</sub> 

- cấu trúc chung của P<sub>i</sub> thực hiện đoạn mã găng CS<sub>i</sub>

![](https://i.imgur.com/BVLoriY.png)

- **viết lại cấu trúc chung của đoạn mã găng**

![](https://i.imgur.com/Hja0Lb0.png)

### - **???giải pháp cho đoạn mã găng cần thoả mãn 3 điều kiện**
![](https://i.imgur.com/xv6ozv1.png)

## Semaphore
- là một biến nguyên, nếu không tính đến toán tử khởi tạo, chỉ có thể truy cập thông qua hai toán tử nguyên tố là `wait` (hoặc P) và `signal` (hoặc V)
    - P: proberen - kiểm tra
    - V: verhogen - tăng lên
- các tiến trình có thể sử dụng chung semaphore
- các toán tử là nguyên tố để đảm bảo không xảy ra trường hợp như vd đồng bộ hoá

### Cài đặt semaphore cổ điển

![](https://i.imgur.com/RjJjkqB.png)

###  Cài đặt semaphore theo cấu trúc

![](https://i.imgur.com/L5WsglF.png)


### Hạn chế của semaphore
- sử dụng semaphore không đúng cách 
    - có thể dẫn đến bế tắc hoặc lỗi do trình tự thực hiện của các tiến trình
    - gây ra bởi lỗi lập trình hoặc do người lập trình không cộng tác

## Cơ chế monitor (giám sát)
- Monitor là một cách tiếp cận để đồng bộ hóa các tác vụ trên máy tính khi phải sử dụng các tài nguyên chung. Gồm:
    - Tập các procedure thao tác trên tài nguyên chung
    - Khóa loại trừ lẫn nhau
    - Các biến tương ứng với các tài nguyên chung
    - Một số các giả định bất biến nhằm tránh các tình huống tương tranh
- được nghiên cứu phát triển để khắc phục các hạn chế của semaphore 

## Monitor type

- Một kiểu (type) hoặc kiểu trừu tượng (abstract type) gồm có các dữ liệu private và các phương thức public 
- Monitor type được đặc trưng bởi tập các toán tử của người sử dụng định nghĩa 
- Monitor type có các biến xác định các trạng thái; mã lệnh của các procedure thao tác trên các biến này

### Cấu trúc một monitor type

![](https://i.imgur.com/xAKB4ck.png)

### Cách sử dụng monitor
- nó được cài đặt sao cho chỉ có một tiến trình được hoạt động trong monitor (loại từ lẫn nhau)
- chưa đủ mạnh để xử lý mọi trường hợp đồng bộ hoá => cần thêm cơ chế "tailor-made" về đồng bộ hoá
- các trường hợp đồng bộ hoá "tailor-made" sử dụng kiểu condition

### Kiểu condition
![](https://i.imgur.com/bYBuNdb.png)

### Đặc điểm của x.signal()
- chỉ đánh thức duy nhất một tiến trình đang chờ
- nếu không có tiến trình chờ, x.signal() không có tác dụng gì
- x.signal() khác với signal trong semaphore cổ điển: signal cổ điển luôn làm thay đổi trạng thái ( giá trị ) của semaphore

### Signal wait/continue

![](https://i.imgur.com/kzWGR8z.png)


## Bế tắc (Deadlock)
- xuất hiện khi hai hay nhiều "hành động" phải chờ một hoặc nhiều hành động khác để kế thúc, nhưng không bao giờ thực hiện được
- máy tính: Bế tắc là tình huống xuất hiện khi hai tiến trình phải chờ đợi nhau giải phóng tài nguyên hoặc nhiều tiến trình chờ sử dụng các tài nguyên theo một “vòng tròn” (circular chain)

### Quy trình sử dụng tài nguyên
- xin phép sử dụng (request)
- sử dụng tài nguyên (use)
- giải phóng tài nguyên sau khi sử dụng (release)

### Điều kiện để có deadlock
- loại trừ lẫn nhau (mutual exclusion)
    - một tài nguyên bị chiếm bởi một tiến trình và không tiến trình nào khác có thể sử dụng tài nguyên này `
- giữ và chờ (hold and wait)
    - Một tiến trình giữ ít nhất một tài nguyên và chờ một số tài nguyên khác rỗi để sử dụng. Các tài nguyên này đang bị một tiến trình khác chiếm giữ
- không có đặc quyền (preemption)
    - Tài nguyên bị chiếm giữ chỉ có thể rỗi khi tiến trình “tự nguyện” giải phóng tài nguyên sau khi đã sử dụng xong
- chờ vòng (circular wait)
    - Một tập tiến trình {P0 , P1 , ..., Pn } có xuất hiện điều kiện “chờ vòng” nếu P0 chờ một tài nguyên do P1 chiếm giữ, P1 chờ một tài nguyên khác do P2 chiếm giữ, ..., Pn-1 chờ tài nguyên do Pn chiếm giữ và Pn chờ tài nguyên do P0 chiếm giữ

### Đồ thị cấp phát tài nguyên (Resource allocation graph)

![](https://i.imgur.com/Fsin2is.png)

![](https://i.imgur.com/6fbPsW1.png)

![](https://i.imgur.com/XNLFrQR.png)

![](https://i.imgur.com/z8H7k3m.png)

### Các phương pháp xử lý bế tắc

- Sử dụng một giao thức để hệ thống không bao giờ rơi vào trạng thái bế tắc: Deadlock prevention (ngăn chặn bế tắc) hoặc Deadlock avoidance (tránh bế tắc)
- Có thể cho phép hệ thống bị bế tắc, phát hiện bế tắc và khắc phục nó
- Bỏ qua bế tắc, xem như bế tắc không bao giờ xuất hiện trong hệ thống

## Ngăn chặn bế tắc
- là phương pháp xử lý bế tắc, không cho nó xảy ra bằng cách làm cho ít nhất một điều kiện cần của bế tắc là C1, C2, C3 hoặc C4 không đuộc thoả mãn (không xảy ra)
- ngăn chặn bế tắc theo phương phá này có tính chất tính (statically)

### Ngăn chặn "loại trừ lẫn nhau"
- C1 (Loại trừ lẫn nhau): là điều kiện bắt buộc cho các tài nguyên không sử dụng chung được → Khó làm cho C1 không xảy ra vì các hệ thống luôn có các tài nguyên không thể sử dụng chung được
### Ngăn chặn "giữ vằ chờ"
- C2 (Giữ và chờ): Có thể làm cho C2 không xảy ra bằng cách đảm bảo:
    - Một tiến trình luôn yêu cầu cấ phát tài nguyên chỉ khi nó không chiếm giữ bất kỳ một tài nguyên nào, hoặc
    - Một tiến trình chỉ thực hiện khi nó được cấp phát toàn bộ các tài nguyên cần thiết

### Ngăn chặn "không có đặc quyền"
- Để ngăn chặn không cho điều kiện này xảy ra, có thể sử dụng giao thức sau: 
    - Nếu tiến trình P (đang chiếm tài nguyên R1 , ..., Rn-1 ) yêu cầu cấp phát tài nguyên Rn nhưng không được cấp phát ngay (có nghĩa là P phải chờ) thì tất cả các tài nguyên R1 , ..., Rn-1 phải được “thu hồi” 
    - Nói cách khác, R1 , ..., Rn-1 phải được “giải phóng” một cách áp đặt, tức là các tài nguyên này phải được đưa vào danh sách các tài nguyên mà P đang chờ cấp phát.
### Ngăn chặn "chờ vòng"
- Một giải pháp ngăn chặn chờ vòng là đánh số thứ tự các tài nguyên và bắt buộc các tiến trình yêu cầu cấp phát tài nguyên theo số thứ tự tăng dần

![](https://i.imgur.com/Xf4aS05.png)

![](https://i.imgur.com/cUGzLUT.png)

### Ưu nhược điểm của ngăn chặn giải pháp bế tắc
- Ưu điểm: ngăn chặn bế tắc (deadlock prevention) là phương pháp tránh được bế tắc bằng cách làm cho điều kiện cần không được thỏa mãn
- Nhược điểm
    - Giảm khả năng tận dụng tài nguyên và giảm thông lượng của hệ thống
    - Không mềm dẻo
## Tránh bế tắc
- phương pháp sử dụng thêm các thông tin về phương thức yêu cầu cấp phát tài nguyên để ra quyết định cấp phát tài nguyên sao cho bế tắc không xảy ra.

## Trạng thái an toàn (safe-state)
- một trạng thái (cấp phát tài nguyên) được gọi là an toàn nếu hệ thống có thể cấp phát tài nguyên cho các tiến trình theo một thứ tự nào đó mà vẫn tránh được bế tắc
- hệ thống ở trong trạng thái an toàn nếu và chỉ nếu tồn tại một thứ tự an toàn (safe-sequence)

### Thứ tự an toàn

![](https://i.imgur.com/pvnLyP9.png)

### Các trạng thái an toàn, không an toàn và bế tắc
- trạng thái an toàn không là trạng thái bế tắc
- trạng thái bế tắc là trạng thái không an toàn
- trạng thái không an toàn có thể là trạng thái bế tắc hoặc không

![](https://i.imgur.com/BSojCV1.png)

### Phát hiện bế tắc
- nếu không áp dụng phòng tránh hoặc ngăn chặn bế tắc thì hệ thống có thể bị bế tắc
- khi đó:
    - cần có thuật toán kiểm tra trạng thái xem có bế tắc xuất hiện hay không
    - thuật toán khôi phục nếu có bế tắc xảy ra
### Khôi phục khi có bế tắc

![](https://i.imgur.com/9mkhv9s.png)

![](https://i.imgur.com/mAXIc4k.png)


## Quản lý bộ nhớ

- chương trình sử dụng địa chỉ ảo
- first-fit: nhanh nhất có thể
- best-fit: tốt nhất có thể

- thời gian truy cập hiệu quả