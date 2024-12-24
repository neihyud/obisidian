# Devops for fresh
- **`/etc`**: chứa tất cả file config
- **`sudo -i`**: vào chế độ sudo
- **config server**: **`vi /etc/netplan/00-installer-config.yml`**
	- **ens33**: dhcp4 = true -> tự động cấp ip -> khi tắt server bật lại thì ip sẽ thay đổi
```js
network:
  ethernets:
    ens33:
      dhcp4: no
      addresses: [172.16.42.128/24]
      gateway4: 172.16.42.2 -> lấy từ vmware edit
      nameservers:
            addresses: [8.8.8.8, 8.8.4.4] -> address google
  version: 2
```
=> run `netplan apply` để áp dụng
- **`free -m`**: trạng thái sử dụng Ram
- **`df -h /`**: check server còn trống bao nhiều disk
- **`hostnamectl set-hostname <name>`**: đổi tên server (-> reboot để áp dụng cập nhập)
- **`etc/hostname`**: config hostname (-> reboot để áp dụng cập nhập)
- **`netstat -tlpun`**: liệt kê các kết nối mạng và các cổng mạng đang hoạt động trên hệ 
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
		- version của công cụ phải lớn hơn version của dự án -> check file config để lấy version của dự án
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
```js
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
```js
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
## Triển khai dự án Backend
1. Cài đặt công cụ cần thiết -> công cụ
	1. how to build ....
2. Xem và sửa file cấu hình -> file cấu hình  (pom.xml)
3. Cài đặt và thiết lập database -> công cụ
	-  config database spring boot (**application.properties**)
	- config port db: **`/etc/mysql/mariadb.conf.d/50-server.cnf`** -> bind address 0.0.0.0 -> restart mariadb
	- Access database:
		- **`mysql -u root`** => **`create database shoeshop;`** => **`create user 'shoeshop'@'%' identified by 'shoeshop';`** (tạo user shoeshop có quyền truy cập vào all db với password shoeshop)
		- **`grant all privileges on shoeshop.* to 'shoeshop'@'%';`**: gán quyền cho user có thể tác động lên db
		- **`flush privileges;`** -> lưu quyền thay đổi
		- **`mysql -h 172.16.42.128 -P 3306 -u shoeshop -p`**
			- -h: host
			- -P: port
			- -u: user
			- -p: password
		- **`source <path_to_.sql>`** -> run file .sql
1. Build dự án -> build
	- **`mvn install --h`**: help
	- **`mvn install -DskipTests=true`**
		- DskipTests: bỏ qua test của maven
	- **`java -jar target/<file.jar>`** -> 
		- build (**`nohup java -jar target/<file.jar>  2>&1 &`**): out put nohub và run background
			- **nohup**: cho phép run process background
			- **2>&1**:  Kết hợp cả thông báo lỗi và thông báo bình thường vào cùng một nơi (thường là file log hoặc terminal).
				- **`2`**: Tượng trưng cho "luồng lỗi chuẩn" (standard error).
				- **`>&1`**: Chuyển hướng luồng lỗi chuẩn (stderr) sang luồng đầu ra chuẩn (stdout).
	- **`ps -ef | grep`**: 
		- **`ps -ef`**: process
		- | : output của câu lệnh trước là input của câu lệnh sau
	- **`kill -p <id_process>`**: -9 - buộc dừng
1. Run dự án -> run
2. Kiểm soát hoạt động -> check

# Gitlab
- create gitlab server: clone server
	- **/etc/netplan/00-installer-config.yml**: config server [[#Devops for fresh]]
- Access gitlab by domain but not have domain -> use add host
	- **`vi /etc/hosts`** : thêm domain (`172.16.42.124 <name>`) -> 
	- `vi /etc/gitlab/gitlabrc`: sửa cầu hình gitlab -> gitlab-ctl reconfigure
	-> cần sửa trên nền tảng để có thể sử dụng hostname tự tạo:
		- linux: /etc/hosts
```js
- curl -s https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash
- sudo apt-get install gitlab-ee=14.4.1-ee.0
```