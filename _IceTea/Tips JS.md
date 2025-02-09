- autocanon: test performance
## Cookie
```
Set-Cookie: sessionId=abc123; Path=/; Domain=example.com; Secure; HttpOnly; SameSite=Strict; Max-Age=3600; Priority=High
```
- `sessionId=abc123`: Cookie lưu thông tin session ID.
- `Path=/`: Có hiệu lực trên toàn bộ website.
- `Domain=example.com`: Có hiệu lực với `example.com` và các subdomain.
- `Secure`: Chỉ gửi cookie qua HTTPS.
- `HttpOnly`: Cookie không thể truy cập qua JavaScript. (document.cookie)
- `SameSite=Strict`: Cookie chỉ được gửi trong các yêu cầu nội bộ.
- `Max-Age=3600`: Cookie tồn tại trong 1 giờ.
- `Priority=High`: Ưu tiên cao khi trình duyệt quản lý bộ nhớ.