![[Pasted image 20250218221142.png]]

Message - and - Query


### **SQL & NoSQL**

1️⃣ **Sự khác nhau giữa SQL và NoSQL? Khi nào chọn cái nào?**  
2️⃣ **Giải thích các loại JOIN trong SQL?**  
3️⃣ **Cách tối ưu query SQL chậm?** (Dùng Index, EXPLAIN, Partitioning...)  
4️⃣ **Cách hoạt động của Transactions trong SQL?**  
5️⃣ **ACID là gì? So sánh với BASE trong NoSQL?**  
6️⃣ **Khi nào nên dùng Redis làm database?**  
7️⃣ **Replication và Sharding trong database là gì?**

⚙ **3. Câu hỏi về System Design**
1️⃣ **Rate Limiting là gì? Cách dùng Redis để giới hạn request?**  
2️⃣ **Thiết kế một hệ thống giống như Instagram?**  
3️⃣ **Làm sao để scale một hệ thống có 1 triệu request/s?**  
4️⃣ **Load Balancer hoạt động như thế nào?**  
5️⃣ **Kafka, RabbitMQ dùng khi nào? So sánh với Redis Pub/Sub?**  
6️⃣ **CQRS là gì? Có nên dùng trong mọi hệ thống không?**
## 🔒 **4. Câu hỏi về Security (Bảo mật)**

1️⃣ **SQL Injection là gì? Cách phòng tránh?**  
2️⃣ **XSS (Cross-Site Scripting) là gì? Làm sao để chặn?**  
3️⃣ **CORS hoạt động thế nào?**  
4️⃣ **OAuth 2.0 là gì? Có những loại Grant Types nào?**  
5️⃣ **DDoS Attack là gì? Cách bảo vệ hệ thống khỏi DDoS?**

## 🚀 **5. Câu hỏi về Performance & Caching**

1️⃣ **Cache là gì? Khi nào nên dùng cache?**  
2️⃣ **So sánh Memcached và Redis?**  
3️⃣ **Cache Invalidation là gì? Các chiến lược (Write-through, Write-back...)?**  
4️⃣ **Làm sao để giảm latency của API?**  
5️⃣ **Lazy Loading và Eager Loading trong ORM là gì?**

## ⚡ **6. Câu hỏi về Microservices & Kubernetes**
1️⃣ **Microservices là gì? So sánh với Monolithic?**  
2️⃣ **Service Discovery trong Microservices hoạt động như thế nào?**  
3️⃣ **Docker hoạt động thế nào? Tại sao lại cần Docker?**  
4️⃣ **So sánh Docker Compose và Kubernetes?**  
5️⃣ **Làm sao để đảm bảo các service trong Microservices giao tiếp ổn định? (Circuit Breaker, Retry...)**

📌 **Ví dụ bài tập Kubernetes**


---

NodeJS, race condition có thể xảy ra khi:
- đọc/ghi file ( lúc này nodejs xử lý đa luồng) => sử dụng file lock
- truy cập vào tài nguyên chung: ví dụ về biến cục bộ => sử dụng mutex
![[assets/Pasted image 20250311230602.png]]