##### **CÁCH ĐỌC MÃ NGUỒN CỦA MỘT EXTENSION BẤT KỲ**

Với bất kỳ extension nào trên store bạn đều có thể đọc được mã nguồn của extension đó và đây là cách mà mình hay dùng.

**B1. Download extension bất kỳ muốn xem mã nguồn.**

**B2. Tìm vị trí extension được lưu trữ**

Để tìm được thư mục lưu trữ bạn truy cập liên kết **chrome://extensions-internals/**

Chrome sẽ trả về cho bạn thông tin của tất cả extension được cài đặt.

Bạn tìm kiếm extension muốn xem bằng cách tìm theo tên hoặc id của extension

Ví dụ trong ảnh bạn sẽ thấy các thông tin về extension được cài đặt như: id, manifest_version, name, permission, path (Đây chính là vị trí lưu trữ source code extension trong máy tính của bạn)

**B3: Truy cập thư mục lưu trữ souce code để có thể nghiên cứu.**

Lưu ý: Thư mục này có thể được ẩn đi nếu bạn truy cập bình thường, bạn có thể tìm cách hiển thị các thư mục ẩn để có thể hiển thị trong trình quản lý thư mục của window.

Source code của extension có thể dễ đọc hoặc khó còn phụ thuộc vào cách mà nhà phát triển sử dụng.

Bằng cách này mình có thể hiểu cơ bản cách extension hoặt động hoặc đơn giản chỉ là những quyền mà extension đang sử dụng.

Nếu bạn đang phát triển extension đây có thể là một gợi ý dành cho bạn.