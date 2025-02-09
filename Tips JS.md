- Bài toán xử lý refresh token khi bị mất: dựa vào ip, divice, location, ...
- check SocketIo trên NestJS viết như thế nào? Xem source sau khi build ra NodeJS viết như thế nào?
- 
---
- mkdir -p name: tạo cùng thư mục  cha nếu nó không tồn tại
- git clone + link + name: đặt tên cho thư mục clone
# Quest
```
=> Tại sao sử dụng JWT mà không phải là session
Không khuyến khích lắm với việc lưu cả AccessToken vào DB hoặc nơi nào đó trên server khi đã lưu RefreshToken dù đồng ý là nó sẽ dễ dàng revoke (nhưng đa số trường hợp là không cần thiết). Thứ 1 nó sẽ khiến cho ứng ụng stateful (trong Rest thì điều này là vi phạm). Thứ 2 thì với mỗi Request lên server đều phải check database hoặc cache để kiếm tra AccessToken (Đây lại là nhược điểm của Session so với JWT, vậy tại sao phải khiến JWT có thêm nhược điểm này ?).
```

- ![[Jwt and Token.png]]
- ![[Pasted image 20250208220048.png]]
# Mysql
- Show processlist: => show các connect đang mở
- Nên sử dụng connectPool thay vì createConnect để tối ưu các connect đang sleep thay vì phải tạo một connect mới
- https://www.youtube.com/watch?v=vjHWkHm4cqo
# Cookies
- `Expires`: xác định thời gian hết hạn (ngày giờ cụ thể), tự động xóa khi hết hạn
- `Max-age`: định nghĩa thời gian tồn tại tính bằng milisecond, ưu tiên hơn expires
- `Domain`: xác định tên miền mà cookie sử dụng (có thể gửi đến subdomain)
- `Secure`: chỉ gửi cookie qua kết nối https
- `HttpOnly`: chỉ có server mới truy cập được cookie, JS không thể truy cập (not use document.cooke) => chống XSS Attack
- **SameSite**
	- Kiểm soát việc gửi cookie cùng với các yêu cầu từ trang khác (Cross-Site).
	- Các giá trị:
	    - **Strict**: Chỉ gửi cookie nếu request từ cùng trang (ngăn CSRF hiệu quả).
	    - **Lax**: Gửi cookie với GET request từ trang khác (an toàn hơn).
	    - **None**: Gửi cookie với mọi request (cần Secure).

# Socket IO
- Sử dụng Socker trong controller
```javascript
// index.js
const io = require('socket.io')(http);
// use _io global
global._io = io;
global._io.on('connection', SocketServices.connection)
app.use(require('./src/routes/chat.route'))
// SocketService.js
class SocketServices{
    //connection socket
    connection( socket ){
        socket.on('disconnect', () => {
            console.log(`User disconnect id is ${socket.id}`);
        })
        // event on here
        socket.on('chat message', msg => {
            console.log(`msg is:::${msg}`)
            _io.emit('chat message', msg)
        })
        // on room..
    }
}
module.exports = new SocketServices();
```
- Có thể tuyền io qua middleware
# LRUCache
- Thường áp dụng trong các bài toán cần tối ưu lưu trữ và truy cập nhanh nhưng bị giới hạn về bộ nhớ
- Nếu cache đầy => xóa dữ liệu cũ và giữ lại dữ liệu hay sử dụng

```javascript
class LRUCache {
  constructor(size) {
    this.size = size
    this.cache = new Map()
  }

  put (key,val) {
    const hasKey = this.cache.has(key) 
    if (hasKey) {
      this.cache.delete(key)
    }
    this.cache.set(key, val)
    if (this.cache.size > this.size) {
      this.cache.delete(this.cache.keys().next.value)
    }
    return true;
  }

  get (key) {
    const hasKey = this.cache.has(key)
    if (hasKey) {
      const val = this.cache.get(key)
      this.cache.delete(key)
      this.cache.set(key, val)
      return val
    }

    return -1
  }

  items () {
    return this.cache.entries()
  }
}
```

# Why async/await in forEach not run sync
- Polyfill forEach không có async await -> không gọi sync => sử dụng for hoặc for in, for of
- Polyfill forEach
```javascript
Array.prototype.forEach = function (callback, thisAgr) {
	for (let i = 0; i < this.length; i++) {
		callback.call(thisAgr, this[i], i, this)
	}
}
```
# Handle image send to server
- Client split file image -> server merge file 
# Test Request đồng thời
- Sử dụng `autocannon`
