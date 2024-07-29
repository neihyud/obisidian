---
title: Redis
tags: [Database]

---

###### tags: `Database`
# Redis
![[Pasted image 20240729210511.png]]

- lưu dữ liệu trong Ram, tuy nhiên khi tắt máy -> mất dữ liệu 
    -> có thể lưu on-disk để có thể khôi phục dữ liệu
    
- Redis has 4 options for persistence:
    - AOF (Append Only File):
        - AOF works like a commit log, recording each write operation to Redis. So when the server is restarted, the write operations can be replayed and the dataset can be reconstructed.
    - RDB (Redis Database):
        - RDB performs point-in-time snapshots at a predefined interval.
    - AOF and RDB.
    - No persistence:
        - Persistence can be entirely disabled in Redis. This is sometimes used when Redis is a cache for smaller datasets, 

![[Pasted image 20240729210518.png]]


# Redlock

- retrycount: khi nó bị lock r hoặc redis lỗi thì nó sẽ cố gắng thực hiện retry nếu vượt quá số lần retry thì sẽ có lỗi
# Cache
- Cache: hi sinh memory/disk để giảm CPU time hoặc network time => tăng tốc độ, giảm tải hệ thống
    - lưu trữ lại các action tốn time => sử dụng lại nhanh hơn
- Vấn đề cache: stale data (data cũ)
- Cache invalidation: 
    - khi dữ liệu thay đổi cần xóa data trong cache và update new data
    - để cache quá lâu, data sẽ bị stale, nếu refresh thường xuyên thì cache trở nên vô nghĩa
# Syntax

- `redis-cli`:
- `KEYS*`: Lấy tất cả các key
- CONNECT metal 6379: kết nối lại cổng 6379

![](https://i.imgur.com/FiqDrSx.png)

# Info

- redis: noSql or non-relational, no table
- redis: có hai cách để lưu dữ liệu

# Why redis
- code ngắn hơn, dễ hiểu hơn, dễ duy trì, nhanh hơn vì k cần đọc db để update
- data luôn trong bộ nhớ, k cần nhập query
- lưu trữ trên RAM
# Structure Redis
- key-value
- structure types: string, list, set, hash, set

# Data types
## Automatic create and remove key
- redis tự động xóa key nếu key trống (trừ Stream) => key exist, tự động thêm key nếu có giá trị
- k thể add cùng key mà khác type
## 
- Keys: 
    - redis là một binary safe, có thể sử dụng binary làm key
## Strings:
- value: <= 512MB
- command 'set': 
    - có tùy chọn: thành công nếu tồn tại, k thành công nếu k tồn tại 
- mget, mset: return array values
- exists: check key exist ? 1 : 0
- type
- ex:(Key expiration): gia hạn key theo seconds
- ttl: check time còn exist của key
- Get
- Set
- Del
## List
- linked list của mộ string values
- rpush(right push)
- lpush(left push)
- lrange(list range: in ra danh sách mảng)
    - return: index - value => index có thể âm -> -1: p tử cuối
- `rpop`(or `lpop`): lấy value and remove
- ltrim: cut and overried list
- llen: check length list
- lmove + name_key + name_key: chuyển một element từ một key này sang một key khác
- brpop: 
    - TH rpop null -> consumer sẽ đợi một khoảng thời gian để gọi lại -> xử lý lệnh vô ích và tạo độ trễ
    - brpop: giống rpop nhưng nó sẽ chặn cho đến khi một element trở lên  khả dụng, hoặc cho đến khi đạt đến thời gian đã chỉ định
    ```javascript=
    > brpop tasks 5
    1) "tasks"
    2) "do_something"

    => nếu tasks trống thì nó sẽ trả về sau 5s để đợi xem có phần tử nào được thêm vào không, k thì nó sẽ trả về luôn
    ```
- khi list empty -> list null
- không thể tạo đề kiểu
## Hashes
- hset
- hmset: set danh sách mảng
## Sets 
- Tập hợp String duy nhất không có thứ tự, có thể tìm ra các tập giao nhau, tập hợp và khác nhau
- sadd + name_key + value + value ...:
- srem: loại bỏ element chỉ định
- smembes: hiển thị tất cả danh sách
- spop: delete một element ngẫu nhiên
- sismember + name_key + value: check name_key có value không
- sinter + name_key + name_key: giao giá trị giữa các key
- sunionstore copy_name name: tạo một cope key
- scard + name_key: số element trong key
## Sorted sets (o(log n))
- mix set and hash
- các element được lấy theo thứ tự 
- zadd + name_key + score + value: sắp xếp theo score
- zrange + name_key + index-start + index-end: lấy giá trị từ start -> end, index có thể âm 
- ZREVRANGE: đảo ngược 
- zrange + name_key + score + value + `withscores`: trả cả về scores
- zremrangebyscore + name_key + value-start + value-end: trả về các giá trị trong phạm vi
- zrank + name_key + value: trả về vị trí của value, sắp xếp theo tăng dần
- zrevrank + name_key + value: trả về vị trí của value, sắp xếp theo giảm dần
- zremrangebylex, zrangebylex: hoạt động phạm vi từ vựng 
## Stream
- là một data structure giống với append-only log, thường sử dụng trong event sourcing, sensor monitoring, notification
- redis cung cấp id duy nhất cho mỗi luồng
- xadd + name_key + * + array key-value: theo obj vào stream, * đại diện cho tự động tạo id
- xadd + name_key + id (tự tạo) + field value
- xlen + name_key: độ dài stream
- xrange + name_key + index_start (có thể là -: min) + index_end(có thể là +:max): 
- xpending: cung cấp khả năng quan sát các
- xack: đánh dấu một tin nhắn đang chờ xử lý là đã được xử lý chính xác
- xgroup: để tạo, hủy, và quản lý các consume group
- xreadgroup đọc một stream thông qua một group consume
- xdel: xóa từng mục ra khỏi stream
- maxlen=0: stream sẽ không bị xóa
### Entry ids
- Gồm hai phần: <millisecondsTime>-<sequenceNumber>
### Listening for new items with XREAD
- một stream có thể có nhiều clients chờ data, 
- tất cả tin nhắn được thêm vào stream vô thời hạn (trừ khi xóa)   
- xác định được các thông báo đang chờ xử lý, khả năng hiển thị lịch sử
> xread count 2 streams mystream 0
- đây là hình thức non-block của xread, count không bắt buộc, streams là bắt buộc
### Group consume
- một group consume nhận dữ liệu từ một stream và phục vụ nhiều người dùng, 
    - một message chỉ gửi cho một người dùng
    - mỗi một consume group có một id không bao giờ được sử dụng. khi consumer yêu cầu một message thì nó chỉ gửi message chưa được gửi trước đó
    - mỗi một consume group theo dõi tất cả message pending (message đã được gửi cho một số người tiêu dùng nhưng vẫn chưa được xác nhận xử lý) 
### Create A Group Consume
   
### Recovering from permanent failures
- xpending mystream mygroup: cung cấp khả năng quan sát các mục đang pending
- xclaim: sao chép thay đổi của người tiêu dùng
## HyperLogLogs
- cấu trúc dữ liệu xác suất, ước tính số lượng của một set
- sử dụng tối đa 12Kb
## Operating on ranges:
    - zrangebycore: sort theo score
    - zremrangebyscore: remove theo score


# Client-side caching

- memory application sử dụng cho local cache không lớn lắm nhưng thời gian truy cập local memory computer nhỏ hơn thời gian truy cập networked service (e.g database) => giảm độ trễ get data, load database
- Ưu điểm client-side caching
    - data có sẵn, độ trễ nhỏ
    - database nhận ít query => serve the same data with a smaller number of nodes

## The Redis implementation of client-side caching
- redis client-side caching support is called `Tracking`, có hai model
    - `default`: server nhớ key client accessed, send invalidation message(có trong memory client) khi key được sửa đổi => tốn memory server side
        - Nếu có 10k ng connect set ask key => server lưu trữ nhiều thông tin => sử dụng hai ý tg để giới hạn memory server-side và cost CPU 
            - 1. Server nhớ danh sách client có thể lưu vào cache một key nhất định trong một table global(call: `Invalidation Table`) Một key mới được chèn vào, serve loại bỏ mục cũ hơn và gửi invalidation message tới client
            - 2. Lưu id client trong invalidation table vì lưu con trỏ tới client sẽ gây ra thu gom rác khi client ngắt kết nối
            
    - `broadcasting`: client send key bằng tiền tố, hậu tố (e.g user:) và nhận thông báo khi match tiền tố

## Hai chế độ kết nối
- 1. data
- 2. invalidation messages

## Opt-in caching
- client chỉ muốn lưu các keys đã chọn vào cache và thông báo rõ ràng tới server: n gì đã lưu và không => cần nhiều băng thông, nhưng giảm data server cần nhớ, invalidation messages(thông báo vô hiệu) client cần nhận
=> Sử dụng OPTIN
![](https://i.imgur.com/ocd9GBQ.png)

>default: keys không lưu vào cache
>để lưu vào cache cần thực hiện lệnh`CLIENT CACHING YES` trước khi lệnh thực để truy xuất dữ liệu
>e.g
![](https://i.imgur.com/TSaQMVp.png)
>CACHING: ảnh hưởng đến lệnh ngay sau nó
>MULTI: ảnh hưởng đến tất cả lệnh trong giao dịch đã được theo dõi

## Broadcastion mode

- main behaviois
    - clients bật client-side caching sử dụng `BCAST` , `PREFIX`: chỉ định tiền tố, nếu không có `PREFIX` client sẽ nhận invalidation messages cho mọi keys, nếu có thì khớp với `PREFIX` mới nhận được invalidation messages
        -  CLIENT TRACKING on REDIRECT 10 BCAST PREFIX object: PREFIX user:
    -  Server không lưu bất cứ gì vào invalidation table mà lưu vào `Prefixes Table`, trong đó mỗi prefixs liên kết với một danh sách khách hàng
    -  mỗi keys khớp prefixes được sửa đổi sẽ gửi invalidation messages cho tất cả client đăng ký với prefix đó
    -  Serve tiêu thụ CPU tỉ lệ thuận với số `prefixes`

## Option NOLOOP
- sử dụng noloop: client thông báo với server không nhận invalidation messagees cho keys mà client sửa đổi

## Những gì lưu Cache
- k lưu keys thay đổi liên tục hoặc hiếm khi được yêu cầu
- lưu keys yêu cầu thường xuyên và thay đổi với tốc độ hợp lý

# Configuration
- redis.conf: file configuration file
- redis.conf chứa lệnh có format `keyword argument1 argument2 ... argumentN`
- config set: set config khi không muốn khởi động lại service n điều này nó sẽ không ghi đè file redis.conf => để ghi đề sử dụng config rewrite
- Giới hạn bộ nhớ(redis.conf): `maxmemory + byte(e.g: maxmemory 100mb)` , maxmemory 0 => k giới hạn bộ nhớ (mạc định hệ thống 64bit, trong 32bit mặc định là 3GB)
- Eviction policies: khi vượt mức maxmemory cho phép sẽ thực hiện eviction policies (tham khảo các eviction policies google)
- Quá trình eviction policies:
    - client run new command => thêm data
    - redis check > maxmemory => loại bỏ keys theo eviction plicies
- Redis lưu trữ dữ liệu trên RAM, và đương nhiên là thao tác đọc/ghi trên RAM, n sau khi tắt máy data sẽ mất và để lưu trữ được dữ liệu ta sử dụng redis persistence
# Tính khả dụng cao với Redis Sentinel
- cung cấp tác vụ như giám sát, thông báo, hoạt động như một configuration provider cho clients
- Khả năng của sentinel
    - Monitoring: liên tục kiếm tra bản sao, bản chính xem có hoạt động như mong đợi không
    - notification: thông báo cho hệ thống, hoặc các chương trình máy tính khác thông qua API rằng nó có hoạt động ổn định với các phiên bản Redis được giám sát không
    - tự động chuyển đổi dự phòng: tự động chuyển đổi sang bản sao nếu bản chính hoạt động không như mong đợi
    - Configuration provider: trả về client địa chỉ của redis server hiện tại
# Redis persistence
- Cách redis ghi data vào disk
- options persistence:
    - RDB (redis database): thực hiện snapshoot tập dữ liệu ở các khoảng thời gian đã chỉ định
    - AOF (append only file): tính liên tục của logs every write operation nhận bởi server, sẽ được phát lại khi khởi động server tạo lại tập dữ liệu ban đầu
    - No persistence: data chỉ tồn tại khi serve chạy
    - RDB + AOF: redis khởi động lại, tệp AOF sử dụng để xây lại dữ liệu ban đầu
- Ưu điểm của RDB
    - RDB làm một file save redis data tại một thời điểm, vd: mỗi giờ lưu 1 bảng trong 24h => dễ dàng khôi phục
    - RDB: khởi động lại nhanh hơn với bộ dữ liệu lớn so với AOF
    - Trên bản sao, RBD hỗ trợ đồng bộ hóa một phần sau khi khởi động lại và chuyển đổi dự phòng
- Nhược điểm
    - RDB: k tốt nếu giảm khả năng mất dữ liệu (vd khi mất điện)
    - RDB fork() nhiều hơn AOF, fork tốn nhiều time => trải nghiệm ng dùng xấu
- Ưu điểm AOF
    - Log AOF là một append-only log nên không có seeks, nếu logs đang viết command một nửa mà lỗi có thể sử dụng tools redis-check-aof
- Nhược điểm:
    - file AOF > RDB
    - AOF có thể chậm hơn RDB
- RDB: snapshoot
    - default: save snapshoot in file `dump.rdb`, có thể config: lưu sau n giây nếu có ít nhất m thay đổi
        >save 60 1000
    - Hoạt động: redis cần dump dữ liệu vào disk
        - redis forks, now have process child and parent
        - child start viết data set vào temporary RDB file
        - khi chile viết xg sẽ thay thế file cũ
- AOF (append only file)
    - bật AOF: `appendonly yes`

# Redis pipelining
- client gửi nhiều lệnh một lúc mà k cần đợi phản hồi từ từng lệnh đơn lẻ, và cuối cùng đọc các câu lệnh trong một bước duy nhất
- Redis là một server TCP client-server
    
# Pub/Sub
- Khi người(pub) không lập trình để gửi message đến một người cụ thể, thay vào đó nó sẽ gửi đến một channel, và nó sẽ gửi đến tất cả người sub channel
# Save
- Save để tạo một bản sao trong file `dump.rdb`
- Khôi phục dữ liệu Redis: di chuyển file dump.rdb vào thư mục của bạn r khởi động lại, `config get dir` để lấy thư mục nơi redis được cài đặt
- bgsave: bắt đầu sao lưu và chạy dưới chế độ nền
# Redis transactions
- là thực hiện một nhóm lệnh trong một lệnh duy nhất
    - thực hiện tuần tự, trong quá trình không có gì xen vào
    - giao dịch redis là nguyên tử nghĩa là tất cả các lệnh hoặc không có lệnh nào được xử lý
- MULTI: bắt đầu một transactions, các lệnh sau câu lệnh này sẽ vào một queu
- EXEC: kết thúc một transactions, thực hiện các câu lệnh sau multi
    
# Redis partitioning
- chia nhỏ data vào trong nhiều instance redis, mỗi một instace chỉ chứa một tập con keys
    