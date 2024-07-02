---
title: Hệ quản trị cơ sở dữ liệu
tags: [School]

---

###### tags: `School`

# Hệ quản trị cơ sở dữ liệu

## MySql
### Cấu trúc tổng quan
![](https://i.imgur.com/rh3uKj6.png)

- **Pluggable Storage Engines**
    - **InnoDB**: ảnh hg bảng khác khi có lỗi, có tốc độ ghi nhanh hơn 
    - **MyISAM**: chỉ ảnh hưởng bảng đó khi có lỗi, sử dụng rộng rãi trong web, kho dữ liệu (data warehouse)
    - **Memory**: tất cả lưu trữ trong RAM cho truy cập nhanh hơn
    - **Merge**: cung cấp khả năng nhóm một chuỗi các bảng MyISAM giống nhau và tham chiếu tới như một đối tượng
    - **Federated**: khả năng tạo CSDL logic từ nhiều server vật lý => môi trg data phân tán

![](https://i.imgur.com/8sH092k.png)

- **Query**: select row1, row2 => lưu vào cache sau đó, select row2, row1 => rẽ nhánh bên phải (p đi qua parser, do 2 query khác nhau)
### Stored Procedure
- làm một code SQL có thể lưu vì vậy nó có thể thực thi nhiều lần, có thể truyền tham số,  Thủ tục được lưu trữ là một phân đoạn của các câu lệnhSQL khai báo được lưu trữ bên trong máy chủ MySQL.
    - **Ưu điểm**: thực thi nhanh hơn, giảm tải băng thông - thay vì gọi nhiều lần đến db thì ta chỉ cần gọi một lần, cho phép sử dụng lại, cơ sở dữ liệu an toàn hơn
    - **Nhược điểm**: 
        - khó khả chuyển, 
        - khi thay đổi thì p kiểm thử lại tất cả,
        - tốn tài nguyên (memory, CPU)
    - nên sử dụng khi có phép toán lặp đi lặp lại nhiều lần, chương trình ưu tiên về tốc độ

### DDL và DML
![](https://i.imgur.com/Lw9qmYJ.png)


### Stored Procedure vs Function
![](https://i.imgur.com/qi88Ywi.png)

### Thực thi trên SQL
![](https://i.imgur.com/DoSUByu.png)

- nếu tìm thấy trong shared pool check thì sd soft parsing
- Optimizer: tập các quy trình quyết định đường thực thi mà DBMS sẽ thực hiện các truy vấn

### Một số đặc trưng của SQL
- hỗ trợ CSDL lớn, bảng lớn (256T) lưu được n chưa chắc xử lý, xử lý được còn phụ thuộc vào nhiều bảng có kết nối với nhau không có n join hay n bảng chồng chéo nhau không
- 64 chỉ mục mỗi bảng, 1-16 cột chỉ mục
- khi thêm sửa xóa thì nó sẽ locking lại dữ liệu
- Có các công cụ phân tích truy vấn và phân
tích không gian
- Máy chủ có thể ở mô hình client-server
hoặc CSDL nhúng
- Quản lý an ninh và phân quyền
- Hỗ trợ kết nối SSL (Secure sockets layers)
- Hỗ trợ CSDL lớn, bảng lớn (lên đến 256
TB)
- Cung cấp kết nối (connectors) đến các
ứng dụng
- 64 chỉ mục mỗi bảng, 1-16 cột mỗi chỉ
mục
- Có các công cụ phân tích truy vấn và phân
tích không gian
- mô hình mysql slave: server die, k thể thay thế (k thể thay thế server)
### MySQL với Oracle
![](https://i.imgur.com/zLhFYdQ.png)

### Một số giải pháp hỗ trợ tính sẵn sàng cao trong mySQL
![](https://i.imgur.com/gvP3t54.png)

- **MySQL Replication**
    - sao chép data từ master server sang 1 or nhiều slave server ( sao chép một chiều)
    - backup data

    ![](https://i.imgur.com/Fg7Aq1T.png)
    -  **binary log**: cơ chế mySQL lưu trữ thay đổi dữ liệu dưới dạng một file log
    -  một replication được tạo, nó sẽ tạo ra 2 luồng
        -  `I/O thread` connect với source MySQL instance  và đọc từng dòng file *binary log*, sau đó copy chúng vào file local trên replica’s server gọi là *relay log*
        -  `SQL thread`: đọc từ relay log sau đó applies them to the replica instance as fast as possible.
- Cơ chế heartbeat: active, pastive

    >**Heartbeat** là quá trình xác định trạng thái hiện tại của các servers đó (node servers) within the replica set. Để làm như vậy, các thành viên của replica set gửi heartbeat (về mặt kỹ thuật là ping) cho nhau sau mỗi **hai giây**. Trong trường hợp, nếu nhịp tim không trở lại (tức là không có phản hồi) trong vòng **10 giây**, thì các thành viên khác của replica set sẽ đánh dấu thành viên quá hạn đó là thành viên không thể truy cập.
**Heartbeat request** là một tin nhắn ngắn kiểm tra trạng thái hiện tại của mọi người. Heartbeat là quá trình để biết trạng thái của thành viên khác, chẳng hạn như ai là người chính, thành viên nào họ cần đồng bộ hóa hoặc nút nào không hoạt động. Hoạt động quan trọng nhất của heartbeat là kiểm tra xem máy chủ chính có hoạt động và có thể truy cập được bởi tất cả các máy chủ phụ không. Trong trường hợp, nếu hầu hết các máy chủ phụ không thể kết nối với máy chủ chính, thì quy trình sẽ tự động hạ cấp máy chủ chính thành máy chủ phụ, sau khi sửa xong máy chủ chính thì nó sẽ đồng bộ lại dữ liệu từ máy chủ phụ sang máy chủ chính

![](https://i.imgur.com/0fpS3wq.png)

### MySQL Index
- Nếu bảng có nhiều index (col1, col2, col3) tìm trong col1 r từ col1 tìm col2 => tìm col3


- key (firstname, lastname) => khác nhau
- đánh index theo nhiều field tìm kiếm hiệu quả theo đúng thứ tự khai báo

- index: tăng hiệu năng cho thao tác đọc
- không nên đánh index khi col k ở trong câu lệnh where

- hash index nhanh hơn b-tree index
- sử dụng b-tree khi so sánh bằng ??? Th sử dụng hashIndex, TH sử dụng b-tree

### MySQL Thread

### Connector, API
- 