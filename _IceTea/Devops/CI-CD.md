![[ci-cd-cycle.png]]

https://about.gitlab.com/blog/2025/01/06/ultimate-guide-to-ci-cd-fundamentals-to-advanced-implementation/
- CI: integrate code -> always check before merge
- Steps
1. **Functional tests**: Kiểm tra xem phần mềm có hoạt động đúng theo mong đợi hay không. Đây thường là các bài kiểm tra tự động mà đội ngũ QA (Quality Assurance) phát triển để đảm bảo tính năng hoạt động đúng. VD: Người dùng có thể thêm sản phẩm vào giỏ hàng và thực hiện thanh toán thành công hay không.
2. **Security scans**: Kiểm tra code có lỗ hổng bảo mật nào không, chẳng hạn như khả năng bị SQL Injection hay [XSS](https://200lab.io/blog/giai-phap-bao-mat-giao-dien-web-ngan-chan-tan-cong-xss-va-csrf-hieu-qua/) (Cross-Site Scripting).
3. **Code quality scans**: Đảm bảo code tuân thủ các tiêu chuẩn, ví dụ như độ dài hàm, cách sử dụng khoảng trắng, và các quy tắc coding style. VD: Tiêu chuẩn như đặt tên biến.
4. **Performance tests**: Kiểm tra xem code có đáp ứng được yêu cầu về hiệu suất không, chẳng hạn như thời gian xử lý yêu cầu hoặc khả năng chịu tải.
5. **License scanning**: Kiểm tra xem tất cả các thư viện hoặc công cụ bạn sử dụng có giấy phép phù hợp hay không, để tránh các vấn đề pháp lý.
6. **Fuzz testing**: Gửi các dữ liệu bất thường, như chuỗi ký tự quá dài hoặc số không hợp lệ, vào ứng dụng để kiểm tra xem nó có bị crash hay gặp lỗi bất ngờ không.