# Devops for fresh
- **`sudo -i`**: vào chế độ sudo
- **config server**: `vi /etc/netplan/00-installer-config.yml`
	- ens33: dhcp4 = true -> tự động cấp ip -> khi tắt server bật lại thì ip sẽ thay đổi
- **`free -m`**: trạng thái sử dụng Ram
- **`df -h /`**: check server còn trống bao nhiều disk
- **`hostnamectl set-hostname <name>`**: đổi tên server (-> reboot để áp dụng cập nhập)
- **`netstat -tlpun`**: 
	- **`-t`**: Hiển thị các kết nối sử dụng giao thức TCP.
	- **`-l`**: Chỉ liệt kê các cổng đang ở trạng thái "lắng nghe" (listening).
	- **`-p`**: Hiển thị tiến trình (process) liên kết với các cổng.
	- **`-u`**: Hiển thị các kết nối sử dụng giao thức UDP.
	- **`-n`**: Hiển thị địa chỉ và cổng dưới dạng số (thay vì chuyển đổi thành tên DNS hoặc tên cổng).
- **`ps -ef`**: các tiến trình chạy trên hệ thống
- **`telnet <server> <port>`**: check server có kết nối ở port nào 

## Quyền truy cập hệ thống
- **`useradd or adduser <name>`**: để thêm một user (adduser thì thêm user chi tiết)
- **`su <username>`**: chuyển sang user khác
- **`vi /etc/passwd`**: xem những thông tin của user
- **`deluser <username>`**: xoá user , nếu +`groupname` thì xóa user ở `groupname` đó
- **`groupadd <namegroup>`**: thêm một group
- **`usermod -aG <namegroup> <username>`**:  thêm user vào group
	- **-a**: append -> thêm vào
	- **-G**: liệt kê các danh sách group của `<username>`, nếu không thêm -G thì sẽ xóa `<username>` ở các group khác
- **`groups + <username>`**: check username thuộc những group nào

## Phân quyền
- **ls -l** -> show chủ sở hữu và nhóm sở hữu
	- **`chown -R <newown>:<newgroupown> <file/folder>`**: đổi quyền sở hữu
		- **-R**: để thay đổi quyền cho cả các file trong folder hoặc file
- Quyền truy cập: ugo - user group own
	- ex: **drwxr-xr-x** - (rwx: read - 4, write - 2, excute - 1)
		- ký tự đầu tiên`d (directory)` - folder , `-` : file 
		- 3 ký tự tiếp theo - quyền sở hữu
		- 3 ký tự tiếp theo - quyền nhóm sở hữu
		- 3 ký tự cuối - quyền của user khác ( không thuộc chủ sở hữu và nhóm sở hữu)
	- **`chmod g=rwx,u=rw,o=- + <folder>`**: thay đổi quyền group thành rwx
	- **`chmod + 750 + <folder/file>`** -> user = rwx, group = rx, o=- 

## Tư duy triển khai dự án
- Triển khai càng nhiều dự án: <font color="#ff0000">sử dụng thư mục riêng</font> và <font color="#ff0000">user riêng cho từng dự án</font>
	- công cụ là gì?
	- file cấu hình ở đâu?
	- làm sao để build? (how to build )
	- làm sao để run?
## Triển khai các dự án
- **`scp <file> <username>@<ip>:<folder_destination>`**: sao chép file ở local tới máy tính từ xa qua giao thức ssh
	- **scp** (Secure Copy) dùng để sao chép tệp giữa các máy tính qua giao thức SSH
	- ex: **scp todolist.zip neihyud@172.16.42.128:/home/neihyud**
	- `mkdir /project`
	- `unzip <file>` ->  `mv /projects`
- Chú ý file config:
	- package.json
- 3 cách để chạy một dự án frontend thông thường
	- sử dụng web-server: 
	- run dưới dạng service
	- run = pm2
- **`0.0.0.0:port`**: tất cả các nơi đều có thể truy cập đến nó
	- nó sẵn sàng nhận kết nối từ bất kỳ giao diện mạng nào trên máy chủ (LAN, Wi-Fi, hoặc cả IP cục bộ). 
### nginx
- **`/etc/nginx`** : tất cả file config lưu trong nginx
	- **sites-available/default**: file mặc định của nginx
- **`nginx -t`**: check systax của file vừa sửa
- **`systemctl restart nginx`**: áp dụng config vừa sửa

=> Để một app chạy trên nginx
- tạo file: **vi conf.d/todolist.conf** -> tạo file **todolist.conf** trong folder **conf.d**
```conf.d
server qi
	listen 8081;
	root /projects/todolist/dist; (path folder tạo ra sau khi run build)
	index index.html => đọc từ file này
	try_files $uri $uri/ /index.html
}
```
- vì mỗi project chạy trên một user và folder khác nhau nên nginx cũng vậy
- **`etc/nginx/nginx.conf`**: check sử dụng user nào (user + <name_user>)
	- để add user nginx vào project -> `usermod -aG todolist www-data` -> systemctl restart nginx hoặc sử dụng `nginx -s reload`
### service
- exit
- **`vi /lib/systemd/system/vision.service`**: 
```
[Service]
Type=simple
User=vision -> vision: name user
Restart=on-failue
WorkingDirectory=/projects/vision
ExecStart=npm run start -- --port=3000
```
- `systemctl daemon-reload`
- `systemctl start vision`
- `systemctl status vision`