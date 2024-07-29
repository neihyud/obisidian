---
title: 'Build, deploy, and scale an E-Commerce app using Microservices built with Node, React, Docker and Kubernetes'

---

# Build, deploy, and scale an E-Commerce app using Microservices built with Node, React, Docker and Kubernetes

# Monolith architecture
![image](https://hackmd.io/_uploads/HkY-DSnIp.png)
![image](https://hackmd.io/_uploads/B1rPPrhUT.png)

![image](https://hackmd.io/_uploads/Hy7BwB2IT.png)

![image](https://hackmd.io/_uploads/By-hvH2Up.png)

![image](https://hackmd.io/_uploads/ByCMtHhUa.png)

![image](https://hackmd.io/_uploads/r1-HtHh86.png)

![image](https://hackmd.io/_uploads/HJRFtBhUT.png)

## Sync and Async in microservice


![image](https://hackmd.io/_uploads/rysGaSnUa.png)

![image](https://hackmd.io/_uploads/BkXwCSnLp.png)


![image](https://hackmd.io/_uploads/SJR1CrhIT.png)
( sử dụng emit hoặc http)

## Event Bus

![image](https://hackmd.io/_uploads/ry00M5xPa.png)


# Architect Base
![image](https://hackmd.io/_uploads/HkaXj_dDa.png)

#
![image](https://hackmd.io/_uploads/rkFjb8IOT.png)

# NodeJS Advanced

![image](https://hackmd.io/_uploads/B13yNQEFa.png)

- nodejs
    - even loop -> single thread
    - libv -> thread pool (default 4 thread) -> run I/O ( các task vụ đòi hỏi p chặn) (run i/o and cryto)

- http not working with thread pool

- Khi chúng ta gọi read file, Node không chỉ đi thẳng đến ổ cứng và ngay lập tức bắt đầu đọc tệp. Thay vào đó, nó nhìn vào tệp trên ổ cứng và cố gắng thu thập một số thống kê về nó, chẳng hạn như kích thước của tệp. Toàn bộ quá trình này bao gồm một lượt đi và về với ổ cứng. Chúng ta đi đến ổ cứng, lấy một số thống kê về tệp, sau đó kết quả trả về cho chương trình của chúng ta.

Sau khi Node có được những thống kê đó, nó hiện đã biết
được kích thước của tệp có thể mong đợi là bao nhiêu, và sau đó nó sẵn sàng thực sự đọc tệp.
Sau đó, Node thực sự quay lại ổ cứng, lấy nội dung của tệp và trả về chúng cho ứng dụng của chúng ta. Cuối cùng, nó gọi lại của chúng ta được kích hoạt.

- giả sử có 1 thread, 1 FS, 4 Hash 
    -> threađ sẽ run lần lượt, fs đọc hard drive vì nó cần thời gian để tính toán và cần làm một công việc khác -> hash được đẩu vào -> nó sẽ in ra cuối cùng
trong TH có 5 thread -> sẽ in ra đầu tiên vì nó k mất công gọi callback khi tính toán hoàn thành

---
Cluster được sử dụng để khởi động nhiều bản sao của Node đang chạy máy chủ của bạn bên trong chúng. Chúng ta không thể "lừa dối" Node vào việc chạy với nhiều thread, nhưng bằng cách khởi động nhiều bản sao, chúng ta có nhiều phiên bản của even loop, điều này hoạt động mơ hồ giống như việc làm cho Node có vẻ multi thread.

![image](https://hackmd.io/_uploads/Hk4JrOStT.png)
![image](https://hackmd.io/_uploads/BJyNB_Bta.png)
