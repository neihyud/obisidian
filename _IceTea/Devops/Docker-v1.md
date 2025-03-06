---
title: Docker
tags: [Devops]

---

# Note
- image tạo với name -> webfood/laravel:v1 : webfood là một kho nhóm các image đó
- nếu có một image individual container -> để connect tới kho đó thì p cùng một network và lúc đó dbhost: name_container được thêm vào
>docker network connect **[name_network webfood]** **[name_container_need_add]**

# Docker
![[docker_ecosystem.png]]
![[Pasted image 20240729183010.png]]
![[Pasted image 20240729183017.png]]
![[Pasted image 20240729183024.png]]

- Kernel is a running software process that governs access between all the programs that are running
    - cầu nối giữa phần cứng và phần mềm trong máy tính

![[Pasted image 20240729183101.png]]

 ## Command
- Run **container**
![[Pasted image 20240729183113.png | 600]]
![[Pasted image 20240729183122.png]]

( FS Snapshot: file symtem snapshot

---

![[Pasted image 20240729183154.png]]


---

![[Pasted image 20240729183213.png]]


![[Pasted image 20240729183225.png]]

---

![[Pasted image 20240729183237.png]]

- docker gửi một Sigterm đến container -> cho phép nó có thời gian kết thúc công việc (lưu file, lưu trạng thái trước khi đóng hoàn toàn)
![[Pasted image 20240729183254.png]]
- shut down immediate

---

![[Pasted image 20240729183318.png]]
- -i: interative -> excec command in container
- -t: ui

---

![[Pasted image 20240729183332.png]]

---
![[Pasted image 20240729183342.png]]

---

![[Pasted image 20240729183354.png]]

![[Pasted image 20240729183404.png]]
![[Pasted image 20240729183413.png]]
![[Pasted image 20240729183418.png]]
![[Pasted image 20240729183436.png]]


---
![[Pasted image 20240729183455.png]]
![[Pasted image 20240729183503.png]]
![[Pasted image 20240729183510.png]]

![[Pasted image 20240729183516.png]]
- khi sử dụng node:alpine -> npm install -> npm start 
    - có node nhưng k có file package.json trong folder trong hard drive docker

---

![[Pasted image 20240729183532.png]]
![[Pasted image 20240729183538.png]]

```javascript=
FROM node:14-alpine
WORKDIR /urs/app

COPY ./package.json  ./
RUN npm install
COPY ./ ./

CMD ["npm", "start"]

=> ta có thể thay đổi thứ tự các command để tăng tốc độ build, vì mỗi khí change ta p build docker file một lần
=> nếu có sự thay đổi ở dưới thì nó sẽ run lại all command ở đoạn dưới
( ví dụ: )

FROM node:14-alpine
WORKDIR /urs/app

COPY ./  ./ => khi change file, docker thấy có sự thay đổi => k thể sử dụng cache => các command ở dưới sẽ chạy ( p npm install lại => lâu hơn)
RUN npm install

CMD ["npm", "start"]
```

![[dockerfile.png]]
![[Pasted image 20240729203743.png]]
![[Pasted image 20240729203814.png]]

=> không cho override folder node_module in docker (-v đầu tiên)

---
---
---
![[Pasted image 20240729203835.png]]


- Docker run first time
![[Pasted image 20240729203905.png]]

- Docker run two time
![[Pasted image 20240729203919.png]]
![[Pasted image 20240729203931.png]]



- **Image**:
![[Pasted image 20240729203940.png]]
![[Pasted image 20240729203944.png | 500]]
![[Pasted image 20240729203948.png]]


## Storage

```javascript=
LOG_SRC=~/example.log; \
LOG_DST=/var/log/nginx/custom.host.access.log; \
docker run -d --name diaweb \
--mount type=bind,src=${CONF_SRC},dst=${CONF_DST} \
--mount type=bind,src=${LOG_SRC},dst=${LOG_DST} \
-p 80:80 \
nginx:latest

--mount với option type=bind, src (xác định vị trí nguồn trên tệp máy chủ),
    dst(destination location on the container file tree)
```

## Containers are not virtualization
- Máy ảo cung cấp phần cứng ảo (hoặc phần cứng trên đó có thể cài đặt hệ điều hành và các chương trình khác). Chúng mất rất nhiều thời gian (thường là vài phút) để tạo và yêu cầu tài nguyên khá lớn vì chúng chạy toàn bộ hệ điều hành cùng với phần mềm mà bạn muốn sử dụng. Máy ảo có thể hoạt động tối ưu khi mọi thứ đã hoạt động, nhưng việc khởi động chậm làm cho chúng không phù hợp cho các kịch bản triển khai theo thời gian thực hoặc phản ứng.
- Các container Docker không sử dụng bất kỳ ảo hóa phần cứng nào. Các chương trình chạy trong các container Docker tương tác trực tiếp với kernel Linux của máy chủ. Nhiều chương trình có thể chạy trong môi trường cô lập mà không cần chạy hệ điều hành dư thừa hoặc chịu đợi đến khi các chuỗi khởi động hoàn chỉnh. Đây là một khác biệt quan trọng.
- Sử dụng công nghệ container đã được tích hợp sẵn trong kernel hệ điều hành của bạn.
## What is image?
- the actual package
- artifact, that can be moved around
- định nghĩa cho một môi trường, là một file read-only, chứa source code, config, libraries cân thiết cho ứng dụng chạy
- Image là 1 trong những đơn vị cơ bản nhất trong Docker. 1 Image sẽ định nghĩa cho 1 môi trường và những thứ có trong môi trường đó. Ứng dụng của ta muốn chạy được thì cần phải có Image

| Images | Container |
| -------- | -------- | 
|  the actual package | actually start the application, 
|| container environment is created
|  artifact, that can be moved around| 
|**not running**| **running**

## What is Container?
- một cách để đóng gói ứng dụng (package application) với tất cả các dependencies và configuration
- portable artifact, shared easy and moved around
- own isolated environment (môi trường biệt lập)
- package with add needed configuration
- run same app with 2 version different without have any conflict
- môi trường chạy app
---
**Về mặt kỹ thuật**
- layers of images
- at the base of mostly of the containerls: mostly linux base image, because small in size
- application image on top
![[Pasted image 20240729204245.png]]
- Một Container là 1 thực thể của Image. Cách hiểu đơn giản nhất đó là: Image các bạn xem như 1 class, còn Container xem như 1 Object được khởi tạo từ class đó.


## Different Image and Container

![](https://i.imgur.com/1TZUIz3.png)


| Container | Image |
| -------- | -------- |
| - is a running environment for **Image**    | Text     |
|port binded: talk to app running inside of container|
|virtual file system


## Docker and Vitual Machine
- both are virtualízation tools

![[Pasted image 20240729204258.png]]

- docker ảo hóa layer applications còn virual machine ảo hóa và and os kernel
- Size: docker image much smaller because they just haveto implement one layer.
- Speed: docker containers start and run much faster than the VMS 
- Compatibility: VM of OS can on any OS host but you can not do that with Docker


## Container Port and Host Port
- nếu container không connect với host port nào thì nó không thể truy cập được bất kì ứng dụng nào
- có thể trùng cổng host post nhưng p khác port container
- **Multiple containers** can run on your host machine
- Your laptop has only certain ports available
- **Conflict when same port** on host machine

## Docker Volumes
- Volume: vì môi trường Docker độc lập nên hệ thống file của ứng dụng chạy trong Docker cũng độc lập, mà thực tế thì hầu như ta luôn cần lưu lại file: lưu trữ DB, lưu trữ log, ảnh,... Do đó để môi trường gốc truy cập được vào file system của ứng dụng chạy trong Docker thì ta cần tới Volume
- Dùng Volume để môi trường gốc có thể nói cho Docker biết rằng: nếu anh có sinh ra file log mới thì anh nhớ lưu cho anh xong thì nhớ tạo 1 "hình chiếu" của file log cho tôi nhé, khi tôi sửa/xóa ở bên tôi (bên ngoài), hoặc anh sửa/xóa ở bên anh thì file log đó đều phải mất ở cả 2 môi trường (gọi đúng thuật ngữ thì là mount từ môi trường ngoài vào trong Docker)
- `Volume`: 
    - để môi trường gốc truy cập vào file system của ứng dụng chạy trong Docker
    - duy trì data được tạo và được sử dụng bởi các docker container
- Data volumes: 
    - data persistence
    - other stateful application
- Folder in physical host file system is **mounted** into the virtual file system of Docker
- Data get automatically replicated
- 3 volume type
    - **host volumes**: bạn quyết định nơi file system host được tham chiếu
    - **anonymous volumes** : each containers a folder is generated that gets mounted
    - **named volume**: reference the volume by **name**

### Volume
    
- Volume: được tạo ra khi container được khởi tạo
- khi container bị xóa thì vấn giữ dược
- Volume trong docker dùng để chia sẻ dữ liệu cho container
- Sử dụng Volume khi nào:
    - gắn một folder trong host vào container
    - chia sẻ dữ liệu giữa host và container
    - backup và restore volume
    
    
### Named volumes
- database là một tệp duy nhất, nếu chúng ta có thể duy trì nó trên host thì ta có thể sử dụng nó ở container tiếp theo bằng cách tạo một `volume` và gắn nó vào thư mục mà dữ liệu được lưu trữ

```dockerfile=
- docker volume create --driver _type <name_volume>
    _type: 
        - "vfs": sử dụng để lưu trữ dữ liệu trên ổ đĩa của host, 
            tuy nhiên hiệu suất thấp hơn so với các driver khác.
        - "overlay2": sử dụng để lưu trữ dữ liệu trong các tệp hệ thống tạo 
            ra từ lớp hệ thống tệp tin của Docker.
        - "btrfs": sử dụng để lưu trữ dữ liệu trên các hệ thống tệp btrfs.
        - "zfs": sử dụng để lưu trữ dữ liệu trên các hệ thống tệp zfs.
        - defaul: local
- docker run -d --driver local <name_volum>:<path_data_container> 
    --name <name_container> <image>
        - docker run -d \
            --volume cass-shared:/var/lib/cassandra/data \ 
            --name cass1 \
            cassandra:2.2
```
![[Pasted image 20240729204308.png]]
![[Pasted image 20240729204313.png]]

- docker volume create _name-db: tạo
```javascript=
- docker volume create todo-db (tạo volume)
-  docker run -dp 3000:3000 -v todo-db:/etc/todos getting-started 
    (sử dụng named volumed và mount nó tới /etc/todos, -v: chỉ định một 
     volume mount, getting-start là tên một images hay là một folder 
     run docker)
    
- docker volume inspect _name: xem data được lưu ở đâu
```
    
- named volumes không quan tâm đến nơi lưu dữ liệu, để kiểm soát điều này ta sử dụng `bind mounts`

## Network
- Mặc định, Docker gồm 3 mạng
    - **driver bridge ( default )**: cung cấp kết nối với tất cả container trên cùng một máy
    - **driver host**: không tạo bất kỳ không gian mạng hoặc tài nguyên mạng đặc biệt nào cho các container được kết nối. Các container trên mạng của máy chủ tương tác với stack mạng của máy chủ giống như các tiến trình không được chứa.
    - **driver null**: Các container được kết nối với mạng none sẽ không có bất kỳ kết nối mạng bên ngoài nào ngoài chính chúng.

- Scope Network: 3 giá trị
    - local
    - global
    - swarm

- Connect Network
![[Pasted image 20240729204338.png]]


![[Pasted image 20240729204347.png]]

## Limiting risk
![[Pasted image 20240729204401.png]]

- `docker stats <name_container>`: xem bộ nhớ container sử dụng

## Docker compose
- tool cấu hình và chạy nhiều docker container cùng lúc, kết nối các container với nhau
- run multiple Docker container
- **Cấu trúc thư mục**:
    1. docker/entrypoint.sh: liệt kê những câu lệnh cần chạy sau khi bật container
    2. Dockerfile
    3. docker-compose.yml: dùng để `khai báo` và `điều phối` hoạt động của các `container` trong project

```dockerfile=
version: "2" -> version docker compose
services:
    mongodb: -> name container
        image: mongo -> dockerhub repo
        ports:
            - 27017:27017 -> host:container
        environment:
            - ... -> dockerhub env and other
        restart: unless-stopped
    
```

- Vì container trong Docker chạy độc lập với môi trường/hệ điều hành gốc nên nếu không khai báo Ports thì nó sẽ chạy ở hệ điều hành gốc chứ không phải ở container 
- **unless-stopped**: chạy service trong mọi TH, nếu dừng có chủ đích thì k restart


### Docker compose command
- `docker-compose -f <_file> up`
- `docker-compose -f <_file> down`: đi qua tất cả container và shut chúng lại
- `docker-compose up -d <name_container>`: start background container (chỉ 1 lần khi mới mở máy tính lên)
- Tạo Makefile: tự động chạy command
```dockerfile=
e.g 

// khi run câu lệnh `make up` thì nó sẽ thực thi dòng lệnh ở dưới,
// các container khai báo sẽ chạy background
up:
    docker-compose up -d mysql redis worker
dev:
    docker-compose run --rm -p 3000:3000 app rails s

```

	![[Pasted image 20240729204418.png]]
- `docker-compose up` sẽ khởi động các service theo thứ tự phụ thuộc, ở đây sẽ là khởi động mysql và redis trước.
- `docker-compose up <app - name in yml>` - tức là khi bạn chỉ khởi động 1 service đơn lẻ thì service mysql và redis vẫn sẽ được khởi động.

### Viết docker-compose

**Example 1**

```dockerfile=
version: '3.5'    // phiên bản docker-compose
services:         // liệt kê các service
    mysql:
        image: mysql:5.7      => chỉ định image để khởi động
        container_name: mysql => chỉ định tên container tùy chỉnh
        restart: always       => default: no, `always` sẽ khởi động 
                                lại nếu code thoát cho biết lỗi fail 
        environment:          => thêm các biến môi trường
          MYSQL_ROOT_PASSWORD: root
        volumes:              => chia sẻ dữ liệu giữa container (máy ảo) 
                                với host (máy thật) hoặc giữa các 
                                container với nhau
          - docker/database:/var/lib/mysql 
            =>khi `container mysql` tạo và lưu dữ liệu sẽ lưu trong thư  
            mục var/lib/mysql của container, nếu container bị xóa 
            thì sẽ mất toàn bộ dữ liệu
    app:
        container_name: app
        build: .  
            => sử dụng khi built từ Dockerfile và 
                Dockerfile thuộc folder docker
        volumes:
          - .:/my_app
        ports:         => cấu hình cổng kết nối (host:container)
          - "3000:3000"
        environment:
          DATABASE_HOST: mysql
          DATABASE_USER_NAME: root
          DATABASE_PASSWORD: root
```

- Để không phải build lại image mỗi lần ta đổi port, ta sẽ khai báo biến môi trường ở docker-compose.yml nhé. Tại sao:
    - env: ở Dockerfile sẽ được khai báo khi build image
    - env trong docker-compose.yml sẽ được khởi tạo**khi container được khỏi tạo** tức là khi chạy `docker-compose up`

## Dockerfile 
![[Pasted image 20240729204449.png]]

- Bất cứ khi nào thay đổi Dockerfile p rebuild lại image vì image không thể bị override
- khi xóa image, nếu bị conflict thì sử dụng command:
    - `docker ls -a | grep <_image>`: grep là command để tìm kiếm
    - `docker rm <_id>`: _id vừa tìm thấy để remove container

- The Dockerfile is a text-based build instruction file that enables us to define the content of the Docker image and automate image creation. The Docker build engine reads the instruction in the Dockerfile line by line and constructs the image as prescribed. In other words, Dockerfile helps us to craft an image repeatedly in an automated fashion. The images created using Dockerfiles are **considered immutable**.
- How it word
![[Pasted image 20240729204500.png]]



### Tạo một Docker Image bằng Dockerfile
example folder: docker-node
![[Pasted image 20240729204507.png]]

- Dockerfile: chứa câu lệnh để tạo mới một Image trong Docker
- Một số lệnh trong Dockerfile:
- **FROM <base_image>:<phiên_bản>**: 
    - bắt buộc ở đầu mỗi file
    - chỉ định Parent Image đang build
    - Docker hub: để lấy thông tin của Image mới nhất
- **WORKDIR**: bên trong Image này tạo folder `app` và chuyển tới `/app`, tg tự **mkdir /app && cd /app**
![[Pasted image 20240729204517.png]]

- **COPY**: có hai dấu chấm => copy ở toàn bộ code ở mtrg gốc (folder **docker-node**) vào bên trong Image ở đường dẫn **/app** 
- **RUN <câu_lệnh>**: chạy khi **build** một Docker Image
- **CMD <câu_lệnh>**: câu lệnh mặc định chạy khi 1 container được khởi tạo từ một image, CMD nhận một mảng bên trong là các câu lệnh muốn chạy
    - **1 Dockerfile** chỉ có **1 CMD**
- **MAINTAINER <tên_tác_giả>**: câu lệnh này dùng để khai báo trên tác giả tạo ra Image, chúng ta có thể khai báo nó hoặc không.
- **ADD <{source}> <{dest}>**: câu lệnh này dùng để copy một tập tin local hoặc remote nào đó (khai báo bằng <source>) vào một vị trí nào đó trên Container (khai báo bằng dest).
![[Pasted image 20240729204526.png]]
![[Pasted image 20240729204539.png]]


- **ENV <tên_biến>**: định nghĩa biến môi trường trong Container.
- **ENTRYPOINT <câu_lệnh>**: định nghĩa những command mặc định, cái mà sẽ được chạy khi container running.
![[Pasted image 20240729204553.png]]


- **VOLUME <tên_thư_mục>**: dùng để truy cập hoặc liên kết một thư mục nào đó trong Container.
![[Pasted image 20240729204615.png]]

- **EXPOSE**: liên kết một port được chỉ định nhằm kết nối process bên trong container và the outside world (i.e. the host).
- **as**: đặt tên cho stage
---
- Copy một hoặc nhiều file khi build: 
```javascript=
COPY app.js .  
# Copy app.js ở folder hiện tại vào đường dẫn ta set ở WORKDIR trong Image

COPY app.js /abc/app.js   
## Kết quả tương tự, ở đây ta nói rõ ràng hơn 
(nếu ta muốn copy tới một chỗ nào khác không phải WORKDIR)

# Copy nhiều file
COPY app.js package.json package-lock.json <path_folder_copy_to_image>
# Ở trên ta các bạn có thể copy bao nhiêu file cũng được, 
<path_folder_copy_to_image> là đích ta muốn copy tới trong Image
```
- Muốn nhiều CMD (khi chạy Dockerfile): sử dụng `ENTRYPOINT`  (thường được lưu trong file `.sh`)



### Build Dockerfile

![[Pasted image 20240729204626.png]]


- `sudo docker build -t <image_name> <context>`: built Docker Image từ Dockerfile 
    - -t: đặt tên image, 
    - {context}: build file trong folder nào(contex nào), dấu '.' chỉ rằng tìm file Dockfile ở folder hiện tại để build
- `sudo docker run -v <forder_in_computer>:<forder_in_container> -p <port_in_computer>:<port_in_container> -it <image_name> /bin/bash`: tạo container từ image
    - `-v`: mount volume, dữ liêụ từ thư mục máy thật có thể được truy cập từ thư mục của máy ảo
    - `-p`: cổng mạng từ máy thật đẫn đến cổng mạng máy ảo đang chạy
    - `-t`: chạy container và mở terminal bằng /bin/bash
    - `-it` (interactive terminal)
    - The **-interactive** or **-i** option starts the container in interactive mode by keeping the STDIN open
    - The **--tty** or **-t** option allocates a pseudo-tty and attaches it to the standard input
## Command
**notes**:
- `docker ps` = list running containers
- `docker ps -a` = list running and stopped container
- `docker exec <name_container> <command>` = chạy tiến trình bổ sung cho một container
- `docker rename <old_container> <new_container>`
- `docker create`: giống với `docker run` chỉ khác là container tạo ra ở trạng thái stopped
- `docker run --rm`: tự động xóa khi stop container

### Image
- `docker images`: List of images downloaded
- `docker images -a`: Hiển thị tất cả các images
- `docker pull <image>` = kéo image từ dockerhub
- `docker run <image>` = pull + start, **start new container from a an image** if not in the container will pull in docker hub
    - have ***tag***: -p, -d, --name
- `docker run -d <image>` = cho phép chạy ngầm
- `docker run -p3000:6701` = 3000:6701 host_port:container_port
- `docker rmi <image_id/name>`: delete image
- `docker images --digests`: Hiển thị tất cả các digests:

### Container
- `docker rm -f <container_id/name>`: xóa một container
- `docker exec -it <container_name> /bin/bash`: truy cập container đang chạy 
- `docker stop <id_container>` = stop container
- `docker start <id_container>` = docker stopped container
    - don't have ***tag***
    
### Debugging a container
- `docker logs <id_or_name>`: docker ps để lấy id or name container
- `docker exec -it <id_container> /bin/bash`: get a terminal of the container (-it: interactive terminal) 
    - `env`: display env của container
    - có thể sử dụng các command: ls, pwd

### Volume
- `docker volume list`: hiển thị tất cả các volume
- `docker volume remove <id_volume>`: remove volume
- `docker volume prune`: remove all volume
- `docker volume prune --filter <condition>`: remove have condition
### Network
- `docker network ls`: list network in docker
- `docker create network <name>`: create new network
- `docker inspect --format '{{ .NetworkSettings.IPAddress }}' <name_container>`: xem địa chỉ IP của container
### Flag
- `--name`: đặt tên cho container
- `-t`: đặt tên cho images
- `-d (--detach)`: chạy ngầm ( chạy background )
- `-p`: host_post:container_port
- `--net`: network
- `\`: multiline
- `docker run --rm`: chạy xong tự xoá
- `-v`: volume
- `-w`: workdir
- `-i`:  Giữ luồng đầu vào chuẩn (stdin) mở cho container ngay cả khi không có bất kỳ terminal nào được gắn kết.
- `-tty`:  Cấp phát một terminal ảo cho container, giúp bạn gửi tín hiệu đến container. => thường dùng cho chương trình dòng lệnh ( thường dùng với -i)
- `-f`: buộc dừng trước khi remove

## Bên trong Container

- nên dùng bản phân phối Alpine để tối giản hóa Image (giảm sizes)
- `docker-compose exec <name_service/container> sh`: bước vào thế giới của Docker (coi docker như một VM), <name_service/container>: bên trong file yml, pos: ở folder contain file yml
- `cat /etc/os-release`: check hệ điều hành trong container



## Câu lệnh thường dùng
- docker-compose -h: help
- docker-compose up: tạo và khởi động containers
- docker-compose down: tắt các container
- docker-compose build: built hoặc rebuilt services
- docker-compose config: hiển thị config
    

    
## Example docker-compose.yml

```javascript=
version: "3.4"

services:
  app:
    image: learning-docker/docker-node-mongo-redis:v1
    volumes:
      - ./:/app # mount từ môi trường gốc vào trong để nếu các bạn thay đổi code thì bên trong sẽ tự động cập nhật
    environment: # phần này ta định nghĩa ở file .env nhé
      - DB_HOST=${DB_HOST}
      - DB_NAME=${DB_NAME}
      - REDIS_HOST=${REDIS_HOST}
      - REDIS_PORT=${REDIS_PORT}
      - PORT=${PORT}
    ports:
      - "${PORT}:${PORT}" # phần này ta định nghĩa ở file .env nhé
    restart: unless-stopped
    depends_on:
        - redis
        - db
  
  db:
    image: mongo:4.4
    volumes:
      - .docker/data/db:/data/db
    restart: unless-stopped
  
  redis:
    image: redis:5-alpine
    volumes:
      - .docker/data/redis:/data
    restart: unless-stopped
```
    
- .docker/data/db:/data/db
    - .docker/data/db: tham chiếu folder trong thư mục gốc (folder này là ta tạo) vào bên trong container ở dường dẫn data/db (để biết được tại sao là đường dẫn data/db trong mongo thì xem mô tả image trong docker hub)
    
- Ở service app chúng ta có để một trường tên là **depends_on**, ý bảo là service app sẽ phụ thuộc vào 2 service db và redis, điều này thực tế sẽ xảy ra như sau:
    - Khi chạy docker-compose up thì service db và redis sẽ khởi động trước service app
    - Khi chạy docker-compose up app thì đồng thời sẽ tạo ra 2 service db và redis (dù khi khởi động ta chỉ nói là "khởi động mỗi service app")
    - Khi chạy docker-compose stop thì service app sẽ bị stop trước 2 service kia
>Mặc dù service app chạy sau db, redis n cx k đảm bảo là db kết nối khi app khởi động vì db tôn một khoảng thời gian để kết nối => cần có option `connectionRetry` để chạy thử cho đến khi nào thành công
    
## Các bản phân phối Linux và cách chọn Image sao cho đúng
- Nên chọn các bản phân phối alpine, dùng alpine sẽ nhẹ hơn nhưng nó sẽ có một vài khác biệt nên khi search google lưu ý với bản alpine

## Data persistent
- Khi ta chạy project bên trong docker container, khi container khởi động lại thì mọi thứ sẽ mất và reset lại như ban đầu
- Do đó ở bài này ta sẽ tạo ra những folder để lưu lại dữ liệu để khi container có khởi động lại thì dữ liệu của ta vẫn sẽ được lưu lại nhé (việc này tiếng anh gọi là persist data), để làm được việc đó thì ta mount những folder chứa dữ liệu này vào trong container dùng volumes nhé 😃

Ở folder gốc, các bạn tạo cho mình folder .docker (để ý dấu chấm ở đầu nhé). Trong .docker ta tạo folder data, trong data ta tạo 2 folder tên là db (cho mongodb) và redis cho redis



## Dockerzire ứng dụng Python, Flash
- Ở Nuxt và Python ở app.run() => app.run(host="0.0.0.0")

## Dockerzire ứng dụng Lavarel
```dockerfile=
# Set master image
FROM php:7.2-fpm-alpine

# Set working directory
WORKDIR /var/www/html

# Install PHP Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Copy existing application directory
COPY . .

```
- k có CMD ở cuối Dockerfile vì bản thân php:7.2 đã chạy cmd rồi

---
```dockerfile=
version: '3.4'
services:

  #PHP Service
  app:
    image: learning-docker/laravel:v1
    restart: unless-stopped
    volumes:
      - ./:/var/www/html

  #Nginx Service
  webserver:
    image: nginx:1.17-alpine
    restart: unless-stopped
    ports:
      - "8000:80"
    volumes:
      - ./:/var/www/html
      - ./nginx.conf:/etc/nginx/conf.d/default.conf

```
- **mount**: hiểu đơn giản là ánh xạ 1 volume từ folder vào bên trong container ở đường dẫn /var/www/html

## Cấu hình Nginx

```dockerfile=
server {
    listen 80;
    index index.php index.html;

    error_log  /var/log/nginx/error.log;
    access_log /var/log/nginx/access.log;

    root /var/www/html/public;

    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass app:9000;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
        fastcgi_hide_header X-Powered-By;
    }

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }
}

```

- **fastcgi_pass app:9000;**: khi có request gửi đến nginx, nginx sẽ chuyển request đó tới PHP-FPM đang lắng nghe ở host tên là app và port 9000.
- **EXOSE 9000**:  nhằm mục đích muốn nói là: tôi chấp nhận cho container khác giao tiếp với tôi ở port 9000
>Mapping port nhằm giúp thế giới bên ngoài giao tiếp được với container
Export port nhằm giúp các container có thể giao tiếp được với nhau, ở trên webserver sẽ cần phải gửi request tới app, nên app phải expose port 9000, nhưng webserver không cần expose vì chẳng có ai gọi đến nó cả.
Ở thế giới bên ngoài sẽ không gọi được vào service app ở port 9000 nhưng webserver thì có thể nhé


## Dockerize nodejs mongodb redis

```dockerfile=
# docker-compose.yml
version: "3.4"

services:
  app:
    image: learning-docker/docker-node-mongo-redis:v1
    volumes:
      - ./:/app # mount từ môi trường gốc vào trong để nếu các bạn thay đổi code thì bên trong sẽ tự động cập nhật
    environment: # phần này ta định nghĩa ở file .env nhé
      - DB_HOST=${DB_HOST}
      - DB_NAME=${DB_NAME}
      - REDIS_HOST=${REDIS_HOST}
      - REDIS_PORT=${REDIS_PORT}
      - PORT=${PORT}
    ports:
      - "${PORT}:${PORT}" # phần này ta định nghĩa ở file .env nhé
    restart: unless-stopped
    depends_on:
        - redis
        - db
  
  db:
    image: mongo:4.4
    volumes:
      - .docker/data/db:/data/db
    restart: unless-stopped
  
  redis:
    image: redis:5-alpine
    volumes:
      - .docker/data/redis:/data
    restart: unless-stopped

```
- Ở service db cho mongodb ta dùng image tên mongo (bản chính thức, các bạn search google là ra nhé)
- Tiếp đó ta mount volume từ folder .docker/data/db ta tạo ở phần trước vào bên trong container ở đường dẫn data/db và lát nữa khi khởi chạy Mongo sẽ tự tìm đến /data/db và load những dữ liệu vào trong database nhé
- /data/db ở phần mô tả dockerhub mongo
- **depends_on**:  ý bảo là service app sẽ phụ thuộc vào 2 service db và redis, điều này thực tế sẽ xảy ra như sau:
    - Khi chạy docker-compose up thì service db và redis sẽ khởi động trước service app
    - Khi chạy docker-compose up app thì đồng thời sẽ tạo ra 2 service db và redis (dù khi khởi động ta chỉ nói là "khởi động mỗi service app")
    - Khi chạy docker-compose stop thì service app sẽ bị stop trước 2 service kia

>Note: mặc dù service app khởi động sau service db nhưng sẽ không đảm bảo service db sẵn sàng ngay thời điểm service app khởi động và kết nối tới nhé, vì service db tốn 1 khoảng thời gian nhỏ từ lúc khởi động đến lúc sẵn sàng cho kết nối. Đó là lí do vì sao ở file app.js mình lại phải có đoạn code connectionRetry để thử kết nối từ NodeJS tới MongoDB cho tới khi nào thành công

=> chưa run được project vì node_modules không có bên trong container, pm2 reset liên tục project của chúng ta

Ồ, really?? Ta đã chạy npm install ở trong Dockerfile rồi cơ mà???

Giải thích: bởi vì trong quá trình dev, ta muốn sửa đổi code thì bên trong container sẽ tự động cập nhật lại, do đó ở docker-compose.yml chúng ta mount volume của service app điều này dẫn tới việc toàn bộ file từ môi trường ngoài sẽ thay thế cho toàn bộ file bên trong container, mà ở môi trường ngoài ta không hề có node_modules.

Giải pháp: yêu cầu ở đây là môi trường ngoài ta phải chạy npm install, nhưng bởi vì môi trường ngoài của ta có cài cái gì ngoài Docker đâu, ta chỉ quan tâm mỗi Docker, vậy làm thế nào có thể chạy được npm install khi không có NodeJS. Các bạn chạy cho mình command sau là được nhé:

```dockerfile=
docker run --rm -v $(pwd):/app -w /app node:13-alpine npm install

# Note trên windows thì command trên chạy như sau:
docker run --rm -v "/$(pwd)":/app -w //app node:13-alpine npm install

```
>This is a Docker command that downloads and runs a Node.js 13-alpine container, installs Node Package Manager (NPM), and runs the "npm install" command in the container's "/app" directory.

![[Pasted image 20240729204649.png]]


### Deploy




---
# Inst
    
![[Pasted image 20240729204810.png]]

![[Pasted image 20240729204816.png]]

```javascript=
- docker stop $(docker ps -aq)
```



## mongdb

- **`docker run -d -p 27017:27017 --name mongodb -v /my/mongodb/data:/data/db mongo`**
>Lệnh này sẽ tải image MongoDB từ Docker Hub và chạy container MongoDB với tên là "mongodb". Container này sẽ lắng nghe trên cổng mạng 27017 và sẽ được ánh xạ vào cổng mạng 27017 trên máy tính của bạn. Lệnh này sẽ ánh xạ thư mục "/data/db" trong container MongoDB sang thư mục "/my/mongodb/data" trên máy tính của bạn.
    
- **`docker inspect --format '{{ .NetworkSettings.IPAddress }}' mongodb`**
>Lệnh này sẽ trả về địa chỉ IP của container MongoDB. Bạn có thể sử dụng địa chỉ này để kết nối đến MongoDB server từ Studio 3T.
Lưu ý rằng nếu bạn muốn lưu trữ dữ liệu của MongoDB, bạn cần ánh xạ thư mục dữ liệu của MongoDB trong container sang một thư mục trên máy tính của bạn. Để làm điều này, bạn có thể sử dụng tham số "-v" trong lệnh "docker run" (ở trên).

## mysql
    
- **`docker run -p 3306:3306 --name my-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql`**

>Lệnh này sẽ tải image MySQL từ Docker Hub và chạy container MySQL Server với tên "my-mysql". Container này sẽ lắng nghe trên cổng mạng 3306 và sẽ được ánh xạ vào cổng mạng 3306 trên máy tính của bạn. Tham số "-e MYSQL_ROOT_PASSWORD=my-secret-pw" sẽ thiết lập mật khẩu cho tài khoản root của MySQL Server là "my-secret-pw".
    
- **`docker inspect --format '{{ .NetworkSettings.IPAddress }}' my-mysql`**
>Lệnh này sẽ trả về địa chỉ IP của container MySQL Server. Bạn có thể sử dụng địa chỉ này để kết nối đến MySQL Server từ MySQL Workbench.
    
