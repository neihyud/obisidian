# Concept
## Container
-  là những **tiến trình được cô lập** cho từng thành phần của ứng dụng. Mỗi thành phần — ứng dụng React ở frontend, engine API Python, và cơ sở dữ liệu — sẽ chạy trong môi trường riêng biệt, hoàn toàn tách biệt với những gì đang chạy trên máy của bạn.
### Điều gì làm cho container trở nên tuyệt vời?
- **Self-contained:** Mỗi container có đầy đủ mọi thứ cần thiết để chạy mà không phụ thuộc vào các thư viện hoặc phần mềm đã cài sẵn trên máy chủ.
- **Isolated:** Vì các container chạy tách biệt, chúng có ảnh hưởng tối thiểu đến host và các container khác, giúp tăng cường bảo mật cho ứng dụng.
- **Independent:** Mỗi container được quản lý độc lập. Xóa một container sẽ không ảnh hưởng đến các container khác.
- **Di động:** Container có thể chạy ở bất kỳ đâu!
## Image
- Một **container image** là một gói chuẩn hóa chứa tất cả các tệp, binary, thư viện và cấu hình cần thiết để chạy một container.
![[docker_image.png | 400]]

### Hai nguyên tắc quan trọng của image:
- **Image là bất biến:** Một khi image được tạo ra, nó không thể bị thay đổi. Bạn chỉ có thể tạo một image mới hoặc thêm các thay đổi lên trên image cũ.
- **Image được tạo thành từ các lớp (layers):** Mỗi lớp là một tập hợp các thay đổi trên hệ thống tệp — thêm, xóa hoặc chỉnh sửa tệp.
## Registry
**Image registry** là **nơi lưu trữ và chia sẻ các container image** tập trung. Registry có thể là:
- **Public (công khai):** Ai cũng có thể truy cập, như **Docker Hub** (registry mặc định của Docker).
- **Private (riêng tư):** Chỉ những người được cấp quyền mới có thể truy cập, phù hợp cho các tổ chức hoặc dự án nội bộ.
- **Kho đăng ký (Registry)** là vị trí tập trung để **lưu trữ và quản lý** các container image, trong khi **kho lưu trữ (Repository)** là một tập hợp các container image có liên quan nằm trong **registry**. Có thể ví von **repository** như một **thư mục** để bạn tổ chức các image theo dự án. Mỗi **repository** chứa **một hoặc nhiều container image**.
![[docker_registry.png | center | 400 ]]
## Docker Compose
- One best practice for containers is that **each container should do one thing and do it well.**
- **Docker Compose** cho phép bạn **định nghĩa tất cả container và cấu hình của chúng trong một tệp YAML duy nhất**.
## Networking
**Mạng trong container** là khả năng cho phép các **container kết nối và giao tiếp với nhau** hoặc với các **ứng dụng bên ngoài Docker**.
- **Mạng mặc định:** Các container có **mạng được kích hoạt sẵn** và có thể **gửi kết nối ra ngoài** mà không cần cấu hình thêm.
### Driver
<table><thead class="bg-gray-light-100 dark:bg-gray-dark-200"><tr><th class="p-2" style="text-align:left">Driver</th><th class="p-2" style="text-align:left">Description</th></tr></thead><tbody><tr><td class="p-2" style="text-align:left"><code>bridge</code></td><td class="p-2" style="text-align:left">The default network driver.</td></tr><tr><td class="p-2" style="text-align:left"><code>host</code></td><td class="p-2" style="text-align:left">Remove network isolation between the container and the Docker host.</td></tr><tr><td class="p-2" style="text-align:left"><code>none</code></td><td class="p-2" style="text-align:left">Completely isolate a container from the host and other containers.</td></tr><tr><td class="p-2" style="text-align:left"><code>overlay</code></td><td class="p-2" style="text-align:left">Overlay networks connect multiple Docker daemons together.</td></tr><tr><td class="p-2" style="text-align:left"><code>ipvlan</code></td><td class="p-2" style="text-align:left">IPvlan networks provide full control over both IPv4 and IPv6 addressing.</td></tr><tr><td class="p-2" style="text-align:left"><code>macvlan</code></td><td class="p-2" style="text-align:left">Assign a MAC address to a container.</td></tr></tbody></table>


# Building Image

## Image Layer
- **Container image** được tạo thành từ nhiều **lớp (layer)**, và mỗi lớp, sau khi được tạo ra, đều là **bất biến (immutable)**
- Mỗi **layer** trong một image chứa **các thay đổi trong hệ thống tệp** — có thể là **thêm**, **xóa**, hoặc **sửa đổi** tệp tin. Hãy thử hình dung một image giả định như sau:
	- **Layer 1:** Thêm các lệnh cơ bản và trình quản lý gói (ví dụ: `apt`).
	- **Layer 2:** Cài đặt **Python runtime** và **pip** để quản lý dependencies.
	- **Layer 3:** Copy tệp **`requirements.txt`** của ứng dụng vào container.
	- **Layer 4:** Cài đặt các thư viện Python từ tệp **`requirements.txt`**.
	- **Layer 5:** Copy mã nguồn ứng dụng vào container.
```.yaml fold title:structure
Layer 1: Base OS + apt
Layer 2: Python runtime + pip
Layer 3: requirements.txt
Layer 4: Installed dependencies
Layer 5: Application source code
```
=>cho phép **tái sử dụng các layer** giữa nhiều image khác nhau. Ví dụ:
- Nếu bạn tạo thêm một ứng dụng Python khác, bạn có thể **dùng lại layer Python runtime** mà không cần build lại từ đầu.
- Điều này giúp **tăng tốc độ build**, **tiết kiệm dung lượng lưu trữ**, và **giảm băng thông** khi phân phối image.
### 🏗️ **Stacking the layers — Cách các layer xếp chồng lên nhau**
**Layering** hoạt động nhờ vào:
- **Content-addressable storage (lưu trữ theo địa chỉ nội dung)**
- **Union filesystem (hệ thống tệp liên kết)**
Hãy cùng xem quá trình diễn ra thế nào nhé!
1. **Tải và trích xuất layer:**  
    Sau khi mỗi lớp được tải xuống, nó được giải nén vào một thư mục riêng trên filesystem của máy chủ.
2. **Tạo hệ thống tệp hợp nhất:**  
    Khi bạn chạy container, Docker sẽ tạo ra **một hệ thống tệp hợp nhất (union filesystem)** bằng cách **xếp chồng các layer lên nhau**, tạo thành **một góc nhìn hợp nhất**.
3. **Thiết lập root filesystem:**  
    Khi container khởi động, thư mục gốc (`/`) của container sẽ trỏ đến thư mục hợp nhất này bằng lệnh **`chroot`**.
4. **Layer ghi đè (write layer):**  
    Ngoài các layer từ image, Docker sẽ tạo thêm **một thư mục dành riêng cho container đang chạy**. Thư mục này giúp:
	- **Lưu các thay đổi khi container chạy** mà không ảnh hưởng đến image gốc.
	- **Chạy nhiều container từ cùng một image** mà không gây xung đột dữ liệu.
## Build, tag, and publish an image
### Tag
 [`docker image tag`](https://docs.docker.com/engine/reference/commandline/image_tag/)

 - Cấu trúc: `[HOST[:PORT_NUMBER]/]PATH[:TAG]`

 - **HOST**: (Tùy chọn) **Địa chỉ registry** nơi lưu trữ image. Nếu không chỉ định, Docker sẽ mặc định sử dụng **docker.io** (registry công khai của Docker).
- **PORT_NUMBER**: (Tùy chọn) **Cổng kết nối** nếu bạn cung cấp hostname.
- **PATH**: **Đường dẫn image**, gồm các thành phần **phân tách bằng dấu gạch chéo**. Với **Docker Hub**, định dạng là:
    - `[NAMESPACE/]REPOSITORY`
    - **Namespace** có thể là **tên người dùng** hoặc **tổ chức**. Nếu không chỉ định, Docker sẽ mặc định sử dụng **library** (là namespace của các Docker Official Images).
	- **TAG**: **Thẻ phiên bản** dễ đọc, thường dùng để **xác định phiên bản** hoặc **biến thể** của image. Nếu không chỉ định, Docker sẽ mặc định dùng **latest**.
## Build Cache
 **Cache invalidation** (vô hiệu hóa cache). Dưới đây là một số tình huống khiến cache bị vô hiệu hóa:
1. **Thay đổi lệnh trong RUN:**  
    Bất kỳ thay đổi nào trong lệnh `RUN` đều **vô hiệu hóa cache của layer đó**. Docker sẽ **kiểm tra sự thay đổi** và nếu phát hiện có sự khác biệt, nó sẽ **chạy lại lệnh** thay vì sử dụng cache cũ.
2. **Thay đổi file trong COPY hoặc ADD:**  
    Docker sẽ **theo dõi các thay đổi** trong thư mục dự án của bạn. Nếu nội dung file, **quyền hạn**, hoặc **thuộc tính** của file thay đổi, Docker sẽ **xóa cache của layer liên quan** và build lại từ đầu.
3. **Hiệu ứng domino khi một layer bị vô hiệu hóa:**  
    Nếu **bất kỳ layer nào bị vô hiệu hóa**, tất cả các **layer phía sau cũng sẽ bị ảnh hưởng**.
## Build context
- **Build context** là tập hợp **các tệp và thư mục** mà quá trình build có thể truy cập. **Tham số vị trí** mà bạn truyền vào lệnh **`docker build`** sẽ xác định **context** mà Docker sử dụng trong quá trình build:
```
docker build [OPTIONS] PATH | URL | -
```
- Docker sẽ gửi **toàn bộ thư mục** (có thể chứa file thừa như `.git`, `node_modules`). => giảm tốc độ build
=> sử dụng .dockerignore để loại bỏ file không cần thiết
# Multi-stage builds 

Trong một quá trình build truyền thống, **tất cả các chỉ thị** trong Dockerfile được thực thi **tuần tự trong cùng một container build**:  downloading dependencies, compiling code, and packaging the application. Tất cả các **layer này** sẽ tồn tại trong image cuối cùng, => **image**, **dung lượng lớn**, và **tăng nguy cơ bảo mật**.

### 🟨 **Multi-stage builds là gì?**
Multi-stage builds cho phép bạn **chia Dockerfile thành nhiều giai đoạn**, mỗi giai đoạn phục vụ **một mục đích riêng**. Hãy tưởng tượng như bạn có thể **chạy từng phần của quá trình build trong các môi trường khác nhau**. Việc **tách biệt môi trường build** và **môi trường runtime** giúp bạn:
- **Giảm kích thước image đáng kể** 🏋️‍♀️
- **Thu nhỏ bề mặt tấn công** (reduce attack surface) và **tăng cường bảo mật** 🔐
### 🟨 **Ví dụ ứng dụng multi-stage build**
- **Ngôn ngữ thông dịch** (JavaScript, Ruby, Python):  
    Bạn có thể **build và minify mã nguồn** ở một stage, sau đó **chỉ copy các file đã tối ưu** sang image runtime nhỏ gọn hơn.

- **Ngôn ngữ biên dịch** (C, Go, Rust):  
    Multi-stage builds cho phép bạn **biên dịch mã** trong một stage, rồi **chỉ copy binary đã biên dịch** vào image cuối cùng.
```dockerfile title:multi-stage
# Stage 1: Build Environment
FROM builder-image AS build-stage 
# Install build tools (e.g., Maven, Gradle)
# Copy source code
# Build commands (e.g., compile, package)

# Stage 2: Runtime environment
FROM runtime-image AS final-stage  
#  Copy application artifacts from the build stage (e.g., JAR file)
COPY --from=build-stage /path/in/build/stage /path/to/place/in/final/stage
# Define runtime configuration (e.g., CMD, ENTRYPOINT) 
```
🔸 **Giai đoạn build:**
	- Dùng **image chứa công cụ build** để biên dịch ứng dụng.
	- Thực thi lệnh build, tạo ra các **artifacts** (ví dụ: file JAR, binary).
🔸 **Giai đoạn runtime:**
	- Dùng **image nhỏ hơn** (chỉ chứa runtime cần thiết).
	- **Chỉ copy kết quả build** từ stage trước qua image runtime.
	- Xác định cấu hình thời gian chạy (CMD hoặc ENTRYPOINT) để khởi động ứng dụng của bạn

# Docker file
 [Dockerfile reference](https://docs.docker.com/engine/reference/builder/).
 ![[dockerfile.png | 600]]

- **FROM**: chỉ đình parent image đang build
- **WORKDIR**: bên trong Image này tạo folder `app` và chuyển tới `/app` =>  **mkdir /app && cd /app
- **COPY**: copy tất cả các files từ project trên máy tính vào **Container image** => path **/app** 
- **RUN**: run khi **build** image  
- **CMD <câu_lệnh>**: câu lệnh mặc định chạy khi 1 container được khởi tạo từ một image, CMD nhận một mảng bên trong là các câu lệnh muốn chạy
    - **1 Dockerfile** chỉ có **1 CMD**
---
- `FROM <image>`  - this specifies the base image that the build will extend.

- `WORKDIR <path>` - this instruction specifies the "working directory" or the path in the image where files will be copied and commands will be executed.
	
- `COPY <host-path> <image-path>` - copy files from the host and put them into the container image.

- `RUN <command>` - the builder to run the specified command.

- `ENV <name> <value>` - sets an environment variable that a running container will use.

- `EXPOSE <port-number>` -  a port the image would like to expose.

- `USER <user-or-uid>` - sets the default user for all subsequent instructions.

- `CMD ["<command>", "<arg1>"]` - sets the default command a container using this image will run.

![[flow_dockerfile.png | 600]]
![[flow_dockerfile_2.png | 600]]
# Docker compose
- `post_start`: run after container started
- `pre_stop`: run before container stopped
- `profiles`: chỉ định những service nào chạy trong một môi trường nhất định
## Sample 1
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
    

## Sample 2
```Dockerfile
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
# Volumn
**Volumes** là một cơ chế lưu trữ cho phép bạn **lưu trữ dữ liệu bên ngoài vòng đời của container**. Bạn có thể tưởng tượng nó như một **đường dẫn tắt (shortcut) hoặc symlink** từ bên trong container ra môi trường bên ngoài.

## **Volume và Bind Mounts**
- **Volumes:** (`-v`)
    - Dùng để **lưu trữ và duy trì dữ liệu lâu dài**, ngay cả khi container bị dừng hoặc xóa.
    - Tệp được lưu **ngoài container**, **Docker quản lý dữ liệu này.** (`-v <name_volume>:<path_folder_data_container>`)
    - Phù hợp khi bạn muốn **bảo vệ dữ liệu** khỏi các thay đổi không mong muốn hoặc cần **chia sẻ dữ liệu giữa nhiều container**.
    - **Ví dụ:** Lưu trữ database hoặc logs của ứng dụng.
- **Bind Mounts:**  (`-v <path_host_data>:<path_container_data>`)
- (`--mount type=bind,source=/HOST/PATH,target=/CONTAINER/PATH,readonly`)
    - Dùng để **gắn kết trực tiếp thư mục hoặc tệp trên máy chủ vào container**.
    - Mọi thay đổi trong thư mục được gắn sẽ **phản ánh ngay lập tức** ở cả máy chủ và container.


# Best Practice
- sử dụng multi-stage
- sử dụng .dockerfileignore

# Command Docker
- `docker image history` command, you can see the command that was used to create each layer within an image.
- `docker exec <name_container> <command>` = chạy tiến trình bổ sung cho một container
- `docker rename <old_container> <new_container>`
- `docker create`: giống với `docker run` chỉ khác là container tạo ra ở trạng thái stopped
- `docker run --rm`: tự động xóa khi stop container
- `docker volume prune`: Remove all unused local volumes
- `docker image prune`: Remove all unused local volumes
- `docker network prune`: Remove all unused networks
### Volumn
- `-v <path_folder_host>:<path_folder_container>` :
	- Khi container chạy, tất cả các tệp được ghi vào thư mục `<folder_container>` trong container sẽ được lưu trữ trong volume.
	- Nếu bạn **xóa container** và tạo một container mới nhưng **dùng lại volume cũ**, thì các tệp vẫn **còn nguyên vẹn**

- `docker volume ls` - list all volumes
- `docker volume rm <volume-name-or-id>` - remove a volume (only works when the volume is not attached to any containers)
- `docker volume prune` - remove all unused (unattached) volumes

- `--mount type=bind,source=<path_host>,target=<path_container>,readonly`
	- sử dụng thêm permission: ro

---
- `docker volume list`: hiển thị tất cả các volume
- `docker volume remove <id_volume>`: remove volume
- `docker volume prune`: remove all volume
- `docker volume prune --filter <condition>`: remove have condition

### Port
- `-p HOST_PORT:CONTAINER_PORT`: 
	- **HOST_PORT**: **Cổng trên máy chủ** nơi bạn muốn nhận lưu lượng.
	- **CONTAINER_PORT**: **Cổng trong container** đang lắng nghe kết nối.
	- Có thể bỏ qua **HOST_PORT** -> auto chọn port ngẫu nhiên
	- Khi publish cổng, nó sẽ **mở ra tất cả các interface mạng** mặc định.
	=> **mọi máy trong mạng** đều có thể truy cập vào ứng dụng đã publish.
### Env
- `docker run --env-file .env postgres env`:
	- xác định file .env cho Dockerfile
	- env -> in ra list env sử dụng trong file
### Image
- `docker images`: List of images downloaded
- `docker images -a`: Hiển thị tất cả các images
- `docker pull <image>` = kéo image từ dockerhub
- `docker run <image>` = pull + start, **start new container from a an image** if not in the container will pull in docker hub
    - have ***tag***: -p, -d, --name
- `docker run -d <image>` = cho phép chạy ngầm
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
# Command Docker Compose
- `docker-compose up`: Create and start containers
- `docker-compose up -d`: Create and start containers in detached mode
- `docker-compose down`: Stop and remove containers, networks, images, and volumes
- `docker-compose logs`: View output from containers
- `docker-compose restart`: Restart all service
- `docker-compose pull`: Pull all image service 
- `docker-compose build`: Build all image service
- `docker-compose config`: Validate and view the Compose file
- `docker-compose scale <service_name>=<replica>`: Scale special service(s)
- `docker-compose top`: Display the running processes
- `docker-compose run -rm -p 2022:22 web bash`: Start web service and runs bash as its command, remove old container.
