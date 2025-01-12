- `sudo passwd <name>`: để đổi mật khẩu
- `sudo nano /etc/pam.d/common-password`: sửa file config password
	- `password requisite pam_unix.so obscure sha512` -> `password requisite pam_unix.so sha512 minlen=1`
# Devops for fresh
- **`/etc`**: chứa tất cả file config
- **`sudo -i`**: vào chế độ sudo
- **config server**: **`vi /etc/netplan/00-installer-config.yaml`**
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
		-  `use <database>`  ->   `source <path_to_.sql>` -> run file .sql
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
- Thêm một server
	- `/etc/netplan/00-installer-config.yml`: 
	- `/etc/hostname`
- create gitlab server: clone server
	- **/etc/netplan/00-installer-config.yml**: config server [[#Devops for fresh]]
- Access gitlab by domain but not have domain -> use add host
	- **`vi /etc/hosts`** : thêm domain (`172.16.42.124 <name>`) -> 
	- `vi /etc/gitlab/gitlab.rb`: sửa cầu hình gitlab -> gitlab-ctl reconfigure
	-> cần sửa trên nền tảng để có thể sử dụng hostname tự tạo:
		- linux: /etc/hosts
```js
- curl -s https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.deb.sh | sudo bash
- sudo apt-get install gitlab-ee=14.4.1-ee.0
```

# CI/CD
![[Pasted image 20241226000536.png]]
- Mỗi stage sẽ xóa sạch code cũ ở stage trước:
		- sử dụng variables: GIT_STRATEGY: clone ở stage: build và GIT_STRATEGY: none ở deploy
- Cài đặt công cụ tự động: ex: Gitlab runner
	- https://docs.gitlab.com/runner/install/linux-repository.html
- Viết config file: .gitlab-ci.yml

- Gitlab: set ci/cd
		- setting -> ci/cd -> runners (vào server - terminal: gitlab-runner register) -> vào file config **`/etc/gitlab-runners/config.f`** chỉnh concurrencies thành 4 -> 
	1. **gitlab-runner run --working-directory /home/gitlab-runner/ --config /etc/gitlab-runner/config.toml --service gitlab-runner  --user gitlab-runner**
	- nên sử dụng để  chạy background -> **nohup gitlab-runner run --working-directory /home/gitlab-runner/ --config /etc/gitlab-runner/config.toml --service gitlab-runner  --user gitlab-runner 2>&1 &**
	2. vào lại gitlab runner, chọn edit runnner vừa chạy bỏ checked "Lock to current project" vì mình cho phép chạy nhiều project trên runner
- Để chạy sudo không cần password, cần config
	- **`visudo`**: dưới config: "user privilege specification" 
		- **giltab-runner ALL=(ALL) ALL **
			- all: áp dụng cho tất cả máy chủ hoặc địa chỉ host
			- (ALL): thực thi với bất kỳ quyền nào
			- ALL: cho phép thực thi tất cả các lệnh
			-> thêm 3 câu lệnh dưới
		- gitlab-runner ALL=(ALL) NOPASSWD: /bin/cp*
		- gitlab-runner ALL=(ALL) NOPASSWD: /bin/chown*
		- gitlab-runner ALL=(ALL) NOPASSWD: /bin/su shopshoe
		- *gitlab-runner ALL=(ALL) NOPASSWD: /bin/kill*
- tạo folder `/datas/shoeshop`: -> chuyển file run (target/file) vào nó
- Các bước thực hiện, sau khi build:
	- chuyển file build ra thư mục riêng: `sudo cp target/file /datas/shoeshop`
	- thay đổi quyền sở hữu: `sudo chown shoeshop. /datas/shoeshop` -> default permission shoeshop=shoeshop
	- chuyển quyền quyền user: `sudo su shoeshop -c "cd /datas/shoeshop; nohup java -jar target/ "` -> chuyển quyền, run câu lệnh
	- run file
- `kill -9 $(ps -ef | grep shoe-ShoppingCart-0.0.1-SNAPSHOT.jar | grep -v grep | awk '{print $2}')`


```
variables:
	- projectname: shoe-ShoppingCart
	- version: 0.0.1-SNAPSHOT
	- projectuser: shoeshop
	- projectpath: /datas/$projectuser

stages:
	- build
	- deploy
	- checklog
	- 
build:
	stage: build
	variables:
	GIT_STRATEGY: clone  #kéo code về, vì mỗi khi run một stage thì code trước bị xóa
	script:
		- mvn install -DskipTests=true
	tags:
		- lab-server
	only:
		- tags
	
deploy:
	stage: deploy
	variables:
	GIT_STRATEGY: none
	script:
		- sudo cp target/$projectname-$version.jar $projectpath
        - sudo chown -R $projectuser. $projectpath
        - sudo kill -9 $(ps -ef | grep shoe-ShoppingCart-0.0.1-SNAPSHOT.jar | grep -v grep | awk '{print $2}') -> kill port đang running
        - sudo su $projectuser -c "cd $projectpath; nohup java -jar target/$projectname-$projectpath.jar >nohup.out 2>&1 &"
	tags:
		- lab-server
	only:
		- tags -> khi nào có tag thì mới run deploy
		
checklog:
	stage: checklog
	variables:
	GIT_STRATEGY: none
	script:
	- sudo su $projectuser -c "cd $projectpath; tail -n 1000 nohup.out"
	tags:
	- lab-server
	only:
	- tags
```


# Docker
- File install docker: install-docker.sh
```
#!/bin/bash

sudo apt update
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce
sudo systemctl start docker
sudo systemctl enable docker
sudo curl -L "https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
docker --version
docker-compose --version
```
- create file `install-docker.sh` -> run file to install docker
## Dockerfile
- dockerfile: 
	- viết cấu hình để đưa source vào container
	- cài đặt các công cụ cần thiết để sẵn sàng khởi chạy
- Command
	- **FROM**: + name docker image -> image chứa một server
	- **WORKDIR**: chỉ định thư mục làm việc
	- **COPY**: copy source vào container
		- copy ..
			- dấu chấm 1: vị trí hiện tại của dockerfile (copy all file cùng cấp)
			- dấu chấm 2: vị trí hiện tại trong container (chỉ định của workdir)
	- **RUN**
	- **ENV**: 
	- **EXPOSE**: `server:container`
	- **CMD**: xác định lệnh và giá trị mặc định
	- **ENTRYPOINT**: giữ nguyên lệnh cố định và cho phép chạy container khi vào cuối của nó
- Tối ưu dockerfile:
	- sử dụng user khác: không sử dụng root
	- image: 
		- docker image dựa trên alpine, 
		- từ các nguồn oficial, verify, sponsor
		- đúng version dự án
		--> maven docker image with java 8 alpine
		-> java docker image version 8: dùng image của aws
	- sử dụng multipe stage, sử dụng công cụ quét image

```dockerfile
## build stage ##
FROM maven:3.5.3-jdk-8-alpine as build -> gán out put bước build vào build

WORKDIR /app

COPY . .

RUN mvn install -DskipTests=true

## run stage ##
FROM amazoncorretto:8u402-alpine-jre
WORKDIR /run 
COPY --from=build /app/target/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar /run/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar -> lấy file build từ bước 'build stage' -> copy vào /run

EXPOSE 8080

ENTRYPOINT java -jar /run/shoe-ShoppingCart-0.0.1-SNAPSHOT.jar

```

```dockerfile
FROM node:18.18-alpine AS build

WORKDIR /app

COPY . .

RUN npm install

RUN npm run build

FROM nginx:alpine

COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]

```

## Registry Server
### Dockub
- **Format**: docker image = domain/project/repo:tag
- docker tag shoeshop:v1 neihyud/shoeshop:v1
### Self-certified: run bằng https
- install: open-ssl
```shell
# apt get update  
# apt-get install openssl  
# mkdir -p /tools/registry/ && cd /tools/registry  
# mkdir data certs  
# openssl req -newkey rsa:4096 -nodes -sha256 -keyout certs/domain.key -subj "/CN=<ip>" -addext "subjectAltName = DNS:<ip>,IP:<ip: 172.16.42.120>" -x509 -days 365 -out certs/domain.crt  
# vi docker-compose.yml
```

```dockerfile
version: '3'
services:
  registry:
    image: registry:2
    restart: always
    container_name: registry-server
    ports:
      - "5000:5000"
    volumes:
      - ./data:/var/lib/registry
      - ./certs:/certs
    environment:
      REGISTRY_HTTP_TLS_CERTIFICATE: ./certs/domain.crt
      REGISTRY_HTTP_TLS_KEY: ./certs/domain.key
```

-> docker-compose up -d
-> `mkdir -p /etc/docker/certs.d/<ip>:<port>`
-> `cp certs/domain.crt /etc/docker/certs.d/<ip>:<port>/ca.crt`
-> `systemctl restart docker`
-> `docker log <ip>:<port>`

- Ở lab-server cũng phải tạo một folder tương tự
	- `mkdir -p /etc/docker/certs.d/<ip>:<port>`
	- ở registry server:`scp certs/domain.crt <user>/<ip>:/home/<user>`
	- `mkdir -p /etc/docker/certs.d/<ip>:<port>` -> ip giống ở registry server
	- `cp /home/neihyud/domain.crt /etc/docker/certs.d/<ip-registry>:<port>/ca.crt`
	- `systemctl restart docker`
	- `docker login <ip-registry-server>:<port>`
	- `docker tag shoeshop:v1 <ip-registry>:<port>/neihyud/shoeshop:v1`
	- `docker push <ip-registry>:<port>/neihyud/shoeshop:v1`
### habor: mua domain, vps

# Jenkins
```jenkins script install
#!/bin/bash

apt install openjdk-11-jdk -y
java --version
wget -p -O - https://pkg.jenkins.io/debian/jenkins.io.key | apt-key add -
sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 5BA31D57EF5975CA
apt-get update
apt install jenkins -y
systemctl start jenkins
systemctl enable jenkins
ufw allow 8080
```
