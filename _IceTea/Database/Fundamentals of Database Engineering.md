![[Pasted image 20241012231442.png]]
![[Pasted image 20241012232606.png]]
	- data được lưu dưới disk dưới dạng file, thuộc 1 hoặc nhiều block of data hoặc phân mảnh nằm rải rác 
- version mới (tuple)

- Tạo DB với 1M records: 
```sql
Inser into grades_org select floor(random()*100) from generate_series (0, 10000000)
```

---

- Khi dùng `GROUP BY`, SQL yêu cầu tất cả các cột trong câu lệnh `SELECT` phải thuộc một trong hai loại:
	- Được nhóm trong `GROUP BY`.
	- Là một phần của hàm tổng hợp (như `COUNT`, `SUM`, `AVG`, v.v.).

- Group by X means put all those with the same value for X in the same row.
- Group by X, Y put all those with the same values for both X and Y in the same row.
---

 - Prevent round when divide -> use:
	 - **CAST**:  (1 as float) / 2
	 - **CONVERT**: (FLOAT , 1) / 2
	 - **Multiply By 1.0**:  (1 * 1.0) / 2
---
- DATEPART: get day, year, second from date
- DATEDIFF
- DATEDIFF_BIG: milliseconds
- CONVERT(DATE, @Datetime): convert date to start date
- DATEFROMPARTS(@Year, @Month, @Day): create date
- DATETIMEFROMPARTS(@Year, @Month, @Day, @Hour, @Minute, @Second, @Milliseconds)
## Database Pages
### A Pool of Pages
- Database thường sử dụng fixed-size pages để store data
	- A pool of page:
		- khi read a row table -> find a page mà chứa row xác dịnh tệp và vị trí nơi page được lưu trữ trên disk -> DB yêu cầu OS read từ tệp ở vị trí cụ thể đó để độc độ dài của page. OS check cache, nếu không có thì thực hiện yêu cầu đọc và pulls the page in memory (cache) to DB dùng
		- DB allocates a pool of memory, often called shared or buffer pool. Các page read từ disk được đặt trong buffer pool -> có thể truy cập vào các row request và các row khác phụ thuộc vào wide the rows 
			=> row càng nhỏ, càng nhiều row trong page và nhiều giá trị thu được trong 1 I/O
		- khi write DB, -> find page where row live -> pulls the page into buffer pool ->  pull the page in the buffer pool and update the row in memory and make a journal entry of the change (often called WAL) persisted to disk
### Page Content
 - **Row-store databases** write rows and all their attributes one after the other packed in the page so that OLTP workloads are better especially write workload.
 - **Column-store databases** write the rows in pages column by column such OLAP workloads that run a summary fewer fields are more efficient. A single page read will be packed with values from one column, making aggregate functions like **SUM** much more effective
## ACID
### Isolation
- [isolation - db](https://en.wikipedia.org/wiki/Isolation_(database_systems))
- Hiện tượng **red**:
	- **Dirty Read**:
		- Nếu một giao dịch đang cập nhật dữ liệu và chưa commit, và một giao dịch khác được phép đọc dữ liệu chưa commit.
		- Cấp độ Repeatable Read đảm bảo rằng một giao dịch không đọc dữ liệu chưa được commit từ giao dịch khác.
		- đọc một cái gì đó mà transacion chưa commit
			- ex: trong tx1 có read giá trị của A nhưng chưa commit, nhưng trong thời gian đó tx2 thực hiện làm thay đổi giá trị của A, nếu ở dưới tx1 đọc lại A thì giá trị không còn giống như lúc ban đầu
		![[db-dirty-read.png]]
	- **Non-repeatable reads (Đọc không lặp lại )**: xảy ra khi một transaction đọc dữ liệu, và sau đó dữ liệu này bị thay đổi bởi một transaction khác trước khi transaction đầu tiên hoàn thành. Cấp độ Repeatable Read đảm bảo rằng các dữ liệu được đọc bởi một giao dịch sẽ không thay đổi trong suốt thời gian giao dịch đó.
		![[db-non-repeatable-reads.png]]
	- **Phantom read**: xảy ra khi một giao dịch thực hiện một truy vấn và sau đó một giao dịch khác chèn hoặc xóa các bản ghi, dẫn đến kết quả khác khi truy vấn tương tự được thực hiện lại trong giao dịch đầu tiên
		=> sử dụng cấp độ **Serializable** để ngăn chặn
		![[db-phantom.png]]
	- **Lost updates**:  I wrote something. But before I commit this write, I try to read what I wrote. Một số transaction khác có thể thay đổi những gì bạn viết vì vậy, nếu quay lại những gì đã viết nó có thể bị mất
		![[db-lost-update.png]]
- **Repeatable Read**:
	- **Khóa record**: các record được đọc bởi transaction sẽ bị lock, tuy nhiên có thể tạo record mới hoặc xóa record cũ    
#### Isolation - Isolation Levels for inflight transactions
- **Read Uncommitted**: Không có sự cô lập, bất kỳ thay đổi nào từ bên ngoài đều có thể nhìn thấy bởi transaction, dù đã commit hay chưa.
- **Read Committed**: Mỗi truy vấn trong một transaction chỉ nhìn thấy các thay đổi đã được commit bởi các transaction khác. ( hay đổi với các transaction khác chưa commit thì nó không thấy những thay đổi)
	-  the transaction will always see the changes it makes regardless of the isolation level. Isolation level only applies to other concurrent transactions
	- có thể ngăn chặn **dirty read** nhưng không thể ngăn chặn **non-repeatable read** và **phantom read**.
- **Repeatable Read**: Giao dịch sẽ đảm bảo rằng khi một truy vấn đọc một hàng, hàng đó sẽ không thay đổi trong suốt thời gian giao dịch đang chạy.
- **Snapshot**: Mỗi truy vấn trong một giao dịch chỉ nhìn thấy các thay đổi đã được cam kết tính đến thời điểm bắt đầu giao dịch. Đây giống như một phiên bản chụp nhanh của cơ sở dữ liệu tại thời điểm đó.
- **Serializable**: Các giao dịch được thực hiện như thể chúng được tuần tự hóa, đảm bảo rằng chúng sẽ không xung đột và mọi thay đổi là nhất quán như khi các giao dịch được thực hiện một cách tuần tự.
	- **Serializable Snapshot Isolation**: khóa toàn bộ, tất cả bản ghi liên quan đến transaction đều bị khóa cho đến khi hoàn thành
	=> hiệu suất giảm, thời gian xử lý lâu do thực hiện tuần tự 
#### Each DBMS implements Isolation level differently
![[isolation-read-phenomena.png]]
#### Database Implementation of Isolation
- **Pessimistic**: Sử dụng các khóa mức hàng (row level locks), khóa bảng (table locks), hoặc khóa trang (page locks) để tránh các cập nhật bị mất (lost updates).
	- **table locks**: là bất kỳ thay đổi nào thực hiện sẽ khiến toàn bộ bảng bị khóa. Điều này rất tốn kém. Tôi đã gặp nhiều tình huống khi các giao dịch của tôi khóa toàn bộ bảng, được gọi là **khóa leo thang (lock escalation)**. Điều này có nghĩa là từ **row level locks** ( khóa hàng), nó đã leo thang lên khóa bảng, và sau đó tất cả các giao dịch khác chỉ đang chờ đợi, cố gắng thay đổi các giá trị khác mà không thể làm được vì bảng đã bị khóa.
    
- **Optimistic**: Không sử dụng khóa, chỉ theo dõi nếu có sự thay đổi và sẽ hủy giao dịch nếu phát hiện thay đổi.
	- nếu có xung đột trong giao dịch -> lỗi (gọi **serializable**), giao dịch thất bại 
    
- **Repeatable Read**: "Khóa" các hàng mà nó đọc, nhưng điều này có thể tốn kém nếu bạn đọc nhiều hàng. PostgreSQL triển khai Repeatable Read bằng cách sử dụng cơ chế snapshot, vì vậy bạn không gặp phải hiện tượng đọc ma (phantom reads) với PostgreSQL trong chế độ Repeatable Read.
    
- **Serializable**: Thường được triển khai với kiểm soát đồng thời lạc quan (optimistic concurrency control), nhưng bạn cũng có thể triển khai nó một cách bi quan (pessimistically) bằng cách sử dụng `SELECT FOR UPDATE`.
=> **Isolation** là kết quả của việc các giao dịch chạy hoàn toàn tách biệt với các giao dịch đồng thời khác.
---
### Atomicity
- Tất cả giao dịch trong transaction phải thành công hoặc thất bại
- Nếu một query thất bại thì tất cả các giao dịch thành công trước phải rollback
- If the database went down prior to a commit of a transaction, all the successful queries in the transactions should rollback
>[!info] Example
>Alice needs to execute 10 DML statements atomically (all or nothing). Alice needs to do the following to achieve true atomicity.
>=> Alice needs to start 1 transaction and issue a commit after executing all statements.
>
> The commit at the end will flush the WAL changes and as a result we will either get a success if the write happened or failure if anything went wrong. If the power went off before the commit we won't get any change, if the power went off right after the commit we will get the correct result. If the power went out during the commit the database will restart and detect the the half/commit state and rollback all changes. If any failure happens during an execution of one of the statements the DB will rollback the transaction and all the changes.

### Consistency
- Consistency in Data
	- defined by user
	- Referential integrity (foreign keys)
- Consistency in Read
	- If a transaction committed a change will a new transaction immediately see the change?
	-  Affects the system as a whole
	- Relational and NoSQL databases suffer from this
	- Eventual consistency
- Dữ liệu được bảo toàn khi sử dụng transaction

>[!info] Example
> - Giả có 2 bảng: like và pic => bảng like có id pic nhưng không có id nào tham chiếu trong bảng pic
> 
> - TH gửi $100 từ account 1 -> account 2. Trong quá trình gửi DB bị lỗi, account 1 bị trừ $100 nhưng
### Durability
- (Durability): đảm bảo rằng hiệu ứng của các giao dịch đã được xác nhận sẽ tồn tại vĩnh viễn, ngay cả trong trường hợp xảy ra lỗi, bao gồm cả các sự cố
- Các thay đổi được thực hiện bởi transaction commit phải được lưu trữ trong một hệ thống lưu trữ không dễ mất dữ mất và đảm bảo tính bền vững
- Durability techniques
	- WAL - Write ahead log
		- lưu trữ một version nén của các thay đổi ( ghi log những thay đổi và tái tạo lại)
	- Asynchronous snapshot:
		- lưu trong trong cache, nhưng sau đó bất đồng bộ trong nền để lưu vào disk
	- AOF
- Lệnh fsync của hệ điều hành buộc các ghi chép luôn được ghi vào đĩa 
- fsync có thể tốn kém và làm chậm quá trình commit

---
- fsync: để đảm bảo rằng các thay đổi dữ liệu đã được commit được lưu trữ bền vững trên đĩa.
- flush-cache: là một lệnh hoặc hành động dùng để buộc hệ điều hành ghi tất cả dữ liệu từ bộ nhớ cache (cache) vào thiết bị lưu trữ.
---
## Database Internal
### Page
- Database không read a single rows, nó read >= 1 page và mỗi page chứa nhiều row
- Mỗi page chứa một size cố định (eg: **postgres**: 8KB, **mysql**: 16KB)
### I/O
- 1 IO có thể fetch >=1 page phụ thuộc vào disk partitions và other factor
- <span style="color: red"><b>Read theo Page và không đọc theo Row</b></span>
### Heap
- The Heap is data structure where the table is stored with all its pages one after another.
- đại diện các page, mọi thứ trong table đều nằm trong Heap
- là một data structure, trong đó bảng được lưu trữ lần lượt với tất cả trang của nó nối tiếp nhau
- Duyệt Heap rất tốn kém, vì ta cần read nhiều data để tìm nó
- Khi đọc trong Heap thì đọc tất cả page để tìm phần tử
=>  cần Index để tìm chính xác phần nào của Heap cần read
### Index
- là một data structure (phổ biến: B-tree) khác Heap, nó có "pointers" tới Heap
- And these pointers is just literally numbers that point to the row ID
- Sau khi tìm giá trị trong Index => vào Heap để tìm chính xác thông tin  (không cần quét từng page)
- Index được lưu dưới dạng page và cost IO to pull the entries of the index 
- được lưu ở file riêng
---
- Bản chất là việc chuyển đổi >=1 colums sang table mới 
	- Table index được **sắp xếp theo thứ tự**
	- Có thể index được nhiều cột cùng lúc, gọi là **composite index**.
	- Giá trị của column là giá trị của một/nhiều column được đánh **index**.
- Thay vì scan table chính. ta **scan table index**
	- không có gì khác biệt: **select *,
	- nếu where column là index -> scan table index (thay vì scan toàn bộ - scan table chính, sử dụng cấu trúc Binary Tree để tiến hành scan. Phổ biến và thông dụng nhất chính là B-Tree (Balanced Tree) index)
- Sau khi sử dụng index, thì mỗi lần update lại data đều phải update lại table index
- Khi thực hiện tạo index -> tạo thêm bảng để lưu giá trị
	- khi insert, table index phải sắp xếp lại -> ảnh hưởng đến tốc độ
	- index chỉ có tác dụng với `where` clause trên column index
### Notes
- Sometimes the heap table can be orgnized around a single index => called clustered or an Index Organized Table 
-  Primary key is usually a clustered index unless otherwise specified.

---
- Postgres: phụ thuộc nặng vào bộ nhớ cache của OS nên IO không nhất thiết p đi qua disk mà có thể truy cập vào cache
## Row and Column Oriented Databases
[Row - Column Oriented Db](https://dataschool.com/data-modeling-101/row-vs-column-oriented-databases/) 
![[row -column.png]]
![[row - db.png]]

### Row-Oriented Database
- data for a single row of a table is stored together in one block or page on disk
	-> hiệu quả khi cần lấy tất cả dữ liệu của một hàng trong một lần (ex: Select queries)
- Một lần đọc vào IO đơn lẻ từ bảng có thể truy xuất **rows** với tất cả các cột của chúng.
- Cần nhiều IOs hơn khi cần tìm một row cụ thể, nhưng khi tìm được thì ta có được tất cả các column của row
- Row-based storage algorithms work by storing the data for each row of a table together in a block or page on disk. When a database needs to read or write a row of data, **it reads or writes the entire block** that contains the row.
- phù hợp OLTP (Online Transaction Processing)
#### Writing
![[Fb - friend.png]]
![[row-db-writing.png]]
#### Reading
- Truy cập nhanh một hàng hoặc một vài hàng
- Number of the disk need to access is larger
- Ex: tính **sum_of_ages** (Facebook Friends) cần load tất cả data vào trong bộ nhớ => lấy data liên quan rồi tổng hợp -> **wasted computing time.**

=> add data fast but get data out can require extra memory and multiple disks to be accessed.
### Column-Oriented Database
- each column of a table is stored separately on disk, which means that all the values for a given column are stored together.
- Một lần đọc vào IO đơn lẻ từ bảng truy xuất nhiều cột với tất cả các hàng phù hợp 
- Khi một hàng mới được thêm vào cơ sở dữ liệu trong hệ thống lưu trữ theo cột, các giá trị cho mỗi cột được thêm vào các tệp cột phù hợp. Điều này có nghĩa là cùng một tệp cột có thể được cập nhật độc lập với các cột khác, điều này có thể mang lại hiệu suất ghi tốt hơn trong một số tình huống. Thêm vào đó, vì dữ liệu cột được lưu trữ cùng nhau, nó có thể được nén hiệu quả hơn, giảm lượng lưu trữ cần thiết trên đĩa.
- Cần ít IO hơn để lấy nhiều giá trị của một cột cụ thể. Nhưng làm việc với nhiều cột đòi hỏi nhiều IO hơn.
- Khi search với Where, nó sẽ tìm thẳng đến **row** search
- phù hợp với OLAP (Online Analytical Processing) 
	- khi tính toán thì các giá trị tính toán ở trong một **row**
#### Writing
![[column-db-writing.png]]
- Nếu lưu trữ data trên nhiều disk thì có nhiều lợi thế đáng kể
![[column-db-disks.png]]
#### Reading

## Database Indexing
- Mỗi **primary key** là một **index**
- <span style="color: red">Trong TH sử dụng like ở câu lệnh Where thì **index** không hoạt động vì index không đáp ứng biểu thức ( hoặc nó không p giá trị duy nhất)</span> 
	- ex: where name like "%Z%"
- Tạo index trong 1 large table tốn rất nhiều thời gian ( trong quá trình tạo, ta không thể write vào db) -> sử dụng `create index concurrently`
- Scan index table  -> scan table chính
	- không phải lúc nào sử dụng index cũng query nhanh hơn vì nó phải xem xét khi sử dụng scan trên 2 table (index  và chính) có nhanh hơn sử dụng **seq scan** trên table chính không? Trong TH seq scan nhanh hơn thì index k có tác dụng mà nó còn làm chậm việc **write**
#### Explain clause
- **Scan table**: duyệt qua từng row
- Đọc plan từ trong ra ngoài
- Các thành phần chính của kết quả
	- **Node** (type)
		- **Seq Scan**: quét tuần tự toàn bộ bảng
		- **Index Scan**: sử dụng index để tìm
		- **Bitmap clause Scan**: sử dụng bitmap index để tìm row
		- **Nested Loop**: kết hợp các bảng bằng cách sử dụng nest loop
		- **Hash Join**: kết hợp bảng sử dụng hash table
		- **Merge Join**: kết hợp sử dụng sắp xếp, kết hợp
	- **Costs**:
		- Start Costs: cost để bắt đầu thực hiện node
			- bao gồm: mở file ( seq scan, index scan, startup cost), khởi tạo cấu trúc (hash table, join nodes), chuẩn bị tài nguyên cấu trúc
		- Total Cost: chi phí tổng để thực hiện node này và các node con
	- Rows: số lượng **row** Postgres nghĩ cần scan
	- **Width**: độ rộng TB mỗi row tính bằng **byte**
	- **Buffers**:
		- `shared hit`: Số lần bộ đệm đã được tìm thấy trong bộ nhớ chia sẻ.
		- `shared read`: Số lần bộ đệm đã được đọc từ đĩa vào bộ nhớ chia sẻ.
		- `shared dirtied`: Số lần bộ đệm đã được thay đổi trong bộ nhớ chia sẻ.
		- `shared written`: Số lần bộ đệm đã được ghi ra đĩa từ bộ nhớ chia sẻ.
### Nest Join
- Join: duyệt từng row rồi map sang table còn lại -> nest loop
- Độ phức tạp O(n^2) -> phù hợp table có số lượng nhỏ

## Concurency Control
##### Shared Lock
- khi read data, data sẽ không bị thay đổi từ lúc đọc đến lúc kết thúc
- vẫn có thể read được data
#### Exclusive lock
- khi write data,  không có bất kỳ ai có thể xen vào quá trình ( không read, write)
- không cho phép bất kì **exclusive lock** hay **shared lock** nào khác được apply trên data đã có **exclusive lock**.
### Deadlock
- A deadlock happens when two processes are or two clients are fighting for one or more resources an either one is waiting for the other process to release the lock on a given resource.
- Ex: giả sử có 2 transaction run cùng run nhưng chưa commit hay rollback
	- transaction1: insert value **10** (primary key)
	- transaction2: insert value **20** (primary key)
		=> success
	- transaction1: insert vaiue **20** đến đấy nó sẽ không thực  thi ngay vì hiện tại đã có giá trị 20 ở transaction2 ( chưa được kết thúc transaction) => nó sẽ đợi transaction 1 thực hiện xong thì mới thực thi tiếp.
		- transaction2: insert value: **10** => deadlock (vì trans1 chờ trans2, trans2 chờ trans1)
### Two-phase Locking
- Two-phase Locking: là ý tưởng về việc khóa cơ sở dữ liệu và giải phóng chúng theo các phase.
	- Phase 1: lock 
	- Phase 2: release
=> release thì không lock lại
Ex: **Double Booking**

**Double Booking**: là khi bạn cố gắng xây dựng một hệ thống đặt chỗ rạp chiếu phim, đúng không?
Và hai người dùng cố gắng đặt cùng một chỗ ngồi tại cùng một thời điểm.
Khi điều đó xảy ra, bạn sẽ gặp phải vấn đề đặt chỗ trùng lặp nếu bạn không có khóa hai pha hoặc một triển khai khác.
Và điều gì xảy ra ở đây là hai giao dịch sẽ được thực hiện cùng một lúc.
Chúng sẽ cùng kiểm tra cơ sở dữ liệu và cả hai sẽ kiểm tra rằng chỗ ngồi còn trống. Hãy đặt nó và cả hai sẽ commit cùng một lúc, dẫn đến vấn đề đặt chỗ trùng lặp.

Bởi vì bạn có hai người trả tiền cho cùng một chỗ ngồi và người cuối cùng sẽ thắng trong trường hợp này, điều đó hoàn toàn không tốt.

Vì vậy, điều chúng ta muốn làm là người đầu tiên thanh toán, chúng ta sẽ ngay lập tức từ chối người thứ hai.

Vì vậy, các bạn, tôi đã nói về vấn đề đặt chỗ trùng lặp từ quan điểm ứng dụng web.

Nếu bạn muốn xem thêm về khung ứng dụng web, hãy xem video tôi đã làm ở đây, nhưng tôi sẽ nói từ quan điểm cấp độ cơ sở dữ liệu ở đây.

Làm sao để chúng ta bắt đầu giao dịch ở đây và chúng ta sẽ bắt đầu giao dịch ở đây.


Vì vậy, điều tôi sẽ làm ở đây là người dùng này sắp đặt chỗ ngồi số 13.

Vì vậy, điều tôi sẽ làm là kiểm tra xem chỗ ngồi có còn trống không.

Chọn tất cả từ bảng chỗ ngồi nơi ID bằng 13 và nó nói rằng, này, nó còn trống và giao dịch thứ hai đồng thời cũng làm điều tương tự từ bảng chỗ ngồi nơi ID bằng 13.

Tuyệt, nó còn trống.

Vì vậy, điều giao dịch này làm là, được rồi, cập nhật, cập nhật bảng chỗ ngồi, chỗ ngồi đã được đặt bằng 1 vì bây giờ nó đã được đặt và tên bây giờ là Hussein nơi ID bằng 13.

Tuyệt vời.

Và điều tương tự xảy ra với giao dịch này.

Vì vậy, cập nhật bảng chỗ ngồi, chỗ ngồi đã được đặt bằng 1 tên bằng Edmund.

Đó là người dùng ở đây nơi ID bằng 13.

Tuyệt vời.

Và bạn sẽ thấy rằng giao dịch này đang chờ.

Đây là hành vi cụ thể của Postgres.

Tuy nhiên, điều gì sẽ xảy ra ở đây là ngay khi tôi cam kết ở đây.

Giao dịch này cũng cam kết, đúng không?

Và khi tôi làm điều đó, tôi sẽ kiểm tra, hãy chọn tất cả từ trước khi cam kết giao dịch khác, tôi sẽ chọn tất cả từ bảng chỗ ngồi ở đây nơi ID bằng 13.
```sql
1. (in transaction)
- T1: get place => 15 empty (exclusive lock)
- T2: get place  (có thực thi nhưng chưa hiển thị do đang lock ở T1)
2.
- T1 -> order 15 (update db)
3.
- T1 -> commit => T2 show result get place (1.) => T2 payment (update db)
-> commit -> T2 đã override name ticket T1
```
- Solution: thêm '**for update**' ở cuối câu lệnh select -> **exclusive lock**

=> Có thể sử dụng flag (isBook) chưa để làm flag check


- Thay vì sử dụng offset, hãy sử dụng ![[Pasted image 20240803000301.png]]
=> nhanh hơn
## Execution Strategy
### 1. **Lập Kế Hoạch Truy Vấn (Query Planning)**

- **Lập Kế Hoạch Tự Động**: PostgreSQL sử dụng trình lập kế hoạch truy vấn để quyết định cách thực thi câu lệnh SQL. Trình lập kế hoạch này sẽ chọn một chiến lược truy vấn tốt nhất dựa trên thống kê về dữ liệu và chỉ số.
    
- **Lập Kế Hoạch Tùy Chỉnh**: Bạn có thể tạo các chỉ số (indexes) trên các cột để cải thiện hiệu suất truy vấn. Trình lập kế hoạch sẽ cân nhắc việc sử dụng các chỉ số này trong quá trình lập kế hoạch.
    

### 2. **Chiến Lược Thực Thi Câu Lệnh**

- **Full Table Scan**: Nếu không có chỉ số phù hợp, PostgreSQL có thể quét toàn bộ bảng để tìm dữ liệu cần thiết. Đây là chiến lược ít hiệu quả hơn đối với bảng lớn.
    
- **Index Scan**: Khi có chỉ số phù hợp, PostgreSQL sẽ thực hiện quét chỉ số để nhanh chóng tìm các hàng liên quan, sau đó truy xuất dữ liệu từ bảng.
    
- **Index Only Scan**: Nếu chỉ số chứa tất cả các cột cần thiết cho câu lệnh truy vấn, PostgreSQL có thể chỉ cần quét chỉ số mà không cần đọc dữ liệu từ bảng.
    
- **Bitmap Index Scan**: PostgreSQL sử dụng bitmap để kết hợp kết quả từ nhiều chỉ số trước khi truy xuất dữ liệu từ bảng. Điều này giúp giảm số lượng truy cập bảng trực tiếp.
    

### 3. **Chiến Lược Kết Nối (Join Strategies)**

- **Nested Loop Join**: Đối với mỗi hàng của bảng bên ngoài, PostgreSQL sẽ tìm kiếm các hàng tương ứng trong bảng bên trong. Phù hợp với các truy vấn nhỏ hoặc khi chỉ số được sử dụng.
    
- **Hash Join**: PostgreSQL tạo một bảng băm từ một bảng và sau đó sử dụng bảng này để tìm các hàng tương ứng trong bảng còn lại. Phù hợp với các bảng lớn.
    
- **Merge Join**: Yêu cầu cả hai bảng đã được sắp xếp theo cột khóa. PostgreSQL sẽ kết hợp các hàng từ hai bảng đã sắp xếp.
    

### 4. **Chiến Lược Phân Tích và Tối Ưu**

- **Phân Tích Câu Lệnh (ANALYZE)**: Cập nhật thống kê về dữ liệu trong bảng để cải thiện khả năng lập kế hoạch của PostgreSQL. Bạn có thể chạy lệnh `ANALYZE` để giúp PostgreSQL hiểu dữ liệu trong bảng.
    
- **Tối Ưu Hóa (VACUUM)**: Xóa các hàng đã bị xóa hoặc cập nhật để giữ cho các chỉ số và bảng sạch sẽ. Giúp duy trì hiệu suất của cơ sở dữ liệu.
    
- **Tuning**: Điều chỉnh cấu hình của PostgreSQL (như `work_mem`, `shared_buffers`, v.v.) để cải thiện hiệu suất.
    

### 5. **Chiến Lược Xử Lý Giao Dịch**

- **Isolation Levels**: PostgreSQL hỗ trợ các mức độ cách ly giao dịch khác nhau (như Read Committed, Serializable) để kiểm soát mức độ cách ly giữa các giao dịch.
    
- **Locking**: PostgreSQL sử dụng các cơ chế khóa để quản lý truy cập đồng thời và tránh các vấn đề như deadlock.
## Vacuum
- Clear **dead tuple**
- Cập nhập statistic information để tối ưu quá trình **query plan**
- Giải quyết vấn đề **over transaction id**
- 
# Partitioning
- chia thành các partitioning, DB tự động gắn partitioning vào table chính ( nó như một bảng cha)
## Vertical Partitioning
## Horizontal Partitioning

# Example
- https://www.sqlclimber.com/assignment/fva93g/customer:-all-orders

### Order

![[Pasted image 20241109172200.png]]
```sql
/*
    Select detail of the last customer order:
    - Product - Format: '{product title} ({brand})'
	- Quantity 
	- UnitPrice
    - TotalPrice
    - Currency
*/
DECLARE @CustomerId INT = 11


SELECT
    Product = p.Title + ' (' + p.Brand + ')'
   ,l.Quantity
   ,l.UnitPrice
   ,TotalPrice = l.Quantity * l.UnitPrice
   ,p.Currency
FROM
(
	-- The last customer order Id.
    SELECT TOP 1
        o.Id
    FROM Eshop.[Order] o
    WHERE o.CustomerId = @CustomerId
    ORDER BY o.OrderDate DESC
) lastOrder
JOIN Eshop.OrderLine l ON l.OrderId = lastOrder.Id
JOIN Eshop.Product p ON p.Id = l.ProductId
JOIN Eshop.Category c ON c.Id = p.CategoryId

```

```sql
/*
    Select all orders for the declared customer:
    - Customer 
    - OrderDate
    - ProductTypes - count of product type 
    - TotalProducts - the quantity of all products
    - TotalPrice - the total price of all products
	The newest orders show up first.
*/
DECLARE @CustomerId INT = 1

SELECT
    c.Customer
   ,o.OrderDate
   ,ProductTypes = COUNT(l.ProductId)
   ,TotalProducts = SUM(l.Quantity)
   ,TotalPrice = SUM(l.Quantity * l.UnitPrice)
FROM Eshop.Customer c
JOIN Eshop.[Order] o ON o.CustomerId = c.Id
JOIN Eshop.OrderLine l ON l.OrderId = o.Id
WHERE c.Id = @CustomerId
GROUP BY c.Customer, o.OrderDate
```


```sql
/*
    Filter products by user filter: @Brand, @PriceFrom, @PriceTo
    If some of the filters are not set, don't apply them.
    
    Select the following columns:
    - Category - Format: {parent category} / {category}
    - Brand
    - Product
    - Price
    - Currency
    The most expensive products show up first.
*/
DECLARE @Brand NVARCHAR(100) = 'Apple'
DECLARE @PriceFrom DECIMAL
DECLARE @PriceTo DECIMAL = 1500

---

SELECT 
    Category = cp.Title + ' / ' + c.Title,
    Brand = p.Brand,
    Product = p.Title,
    Price = p.UnitPrice,
    Currency = p.Currency
FROM Eshop.Product p
JOIN Eshop.Category c ON c.Id = p.CategoryId
JOIN Eshop.Category cp ON c.ParentCategoryId = cp.Id
WHERE 
    (@Brand IS NULL OR p.Brand = @Brand) AND
    (@PriceFrom IS NULL OR p.UnitPrice >= @PriceFrom) AND
    (@PriceTo IS NULL OR p.UnitPrice <= @PriceTo)
ORDER BY p.UnitPrice DESC;
```

```sql
/*
	E-shop wants to expand to new markets:
	- Insert products of the declared category into the table @ExpandingProduct with new currency and recalculated price.
    - All new prices should contain 95 cents/penny, so fix it after recalculation.
*/
DECLARE @ExpandingProduct TABLE
(
	Title NVARCHAR(100)
   ,Description NVARCHAR(1000)
   ,Brand NVARCHAR(100)
   ,CategoryId INT
   ,UnitPrice DECIMAL(18,2)
   ,Currency NVARCHAR(3)
)

DECLARE @Category NVARCHAR(100) = 'Headphones'
DECLARE @NewCurrency TABLE (Currency NVARCHAR(3), ExchangeRate DECIMAL(18, 2))
INSERT INTO @NewCurrency (Currency, ExchangeRate) VALUES ('EUR', 0.89), ('GBP', 0.80)


INSERT INTO @ExpandingProduct
SELECT	
    p.Title
   ,p.Description
   ,p.Brand 
   ,p.CategoryId
   ,UnitPrice = CONVERT(DECIMAL(18,2), FLOOR(p.UnitPrice * cur.ExchangeRate)) + 0.95
   ,cur.Currency
FROM Eshop.Category c
JOIN Eshop.Product p ON p.CategoryId = c.Id
CROSS JOIN @NewCurrency cur
WHERE c.Title = @Category
```


```sql
/*
    E-shop wants to change prices:
    - Calculate new unit prices at table @PreparePrice for the declared category.
    - All new prices higher than 100 dollars should be without cents. Round them up. 
*/
DECLARE @PreparePrice TABLE
(
	ProductId INT
   ,Title NVARCHAR(100)
   ,UnitPrice DECIMAL(18,2)
   ,NewUnitPrice DECIMAL(18,2)
)
INSERT INTO @PreparePrice (ProductId, Title, UnitPrice)
SELECT 
	p.Id
   ,p.Title
   ,p.UnitPrice
FROM Eshop.Product p

DECLARE @ParentCategory NVARCHAR(100) = 'Men''s Accessories'
DECLARE @PriceIncreaseInPercentage INT = 5

C1:
UPDATE pp
SET NewUnitPrice = 
    CASE 
        WHEN (pp.UnitPrice  + pp.UnitPrice * @PriceIncreaseInPercentage/100.0) <= 100 
        THEN CAST(pp.UnitPrice * (100 + @PriceIncreaseInPercentage) AS FLOAT)/100.0
        ELSE CEILING(pp.UnitPrice * (1 + @PriceIncreaseInPercentage/100.0))
    END
FROM @PreparePrice pp
JOIN Eshop.Product p ON pp.ProductId = p.Id
JOIN Eshop.Category c ON p.CategoryId = c.Id
JOIN Eshop.Category pc ON c.ParentCategoryId = pc.Id
WHERE pc.Title = @ParentCategory

C2:
UPDATE pp
SET pp.NewUnitPrice = p.UnitPrice * (100 + @PriceIncreaseInPercentage) / 100
FROM Eshop.Category parentC
JOIN Eshop.Category c ON c.ParentCategoryId = parentC.Id
JOIN Eshop.Product p ON p.CategoryId = c.Id
JOIN @PreparePrice pp ON pp.ProductId = p.Id
WHERE parentC.Title = @ParentCategory

UPDATE pp
SET pp.NewUnitPrice = CEILING(pp.NewUnitPrice)
FROM @PreparePrice pp
WHERE pp.NewUnitPrice > 100

```

---

```sql
/*
    There is declared the flight date.
    Calculate how many years, months, and days have passed since this date till today.
*/
DECLARE @Flight DATE = '1927-05-21'
DECLARE @Today DATE = GETDATE()



DECLARE @Months INT = DATEDIFF(MONTH, @Flight, @Today)

IF DATEADD(MONTH, @Months, @Flight) > @Today
	SET @Months = @Months - 1

SET @Flight = DATEADD(MONTH, @Months, @Flight)

DECLARE @Days INT = DATEDIFF(DAY, @Flight, @Today)

SELECT [Spirit of St. Louis] = 
	'The first solo nonstop transatlantic flight was ' + 
    CONVERT(NVARCHAR, @Months / 12) + ' years, ' + 
    CONVERT(NVARCHAR, @Months % 12) + ' months, and ' + 
    CONVERT(NVARCHAR, @Days) + ' days ago.' 
```

### Flight
![[Pasted image 20241111222755.png]]

```sql
/*
    There is schema Travel with tables: Airport, Flight, and Company.
    Find direct flights from @Origin to @Destination and return flights from @Destination back to @Origin.
    @FlightDate is a date when you want to leave the @Origin.
    @NightInBeijing is a number of nights you want to stay on holiday.
*/
DECLARE @Origin NVARCHAR(100) = 'Los Angeles'
DECLARE @Destination NVARCHAR(100) = 'Beijing'
DECLARE @FlightDate DATE = DATEADD(DAY, 3, GETDATE())
DECLARE @NightInBeijing INT = 10


SELECT 
Flight = Format(f1.DepartureTime, 'dd MMM, HH:mm, ') + UPPER(a1.City)+' -> ' + UPPER(a2.City),
ReturnFlight = Format(f2.DepartureTime, 'dd MMM, HH:mm, ') + UPPER(a2.City)+' -> ' + UPPER(a3.City)
FROM Travel.Flight f1
JOIN Travel.Airport a1 ON a1.Id=f1.DepartureAirportId and a1.City=@Origin and CAST(f1.DepartureTime AS DATE)=@FlightDate
JOIN Travel.Airport a2 ON a2.Id= f1.ArrivalAirportId and a2.City=@Destination
JOIN Travel.Flight f2 ON f2.DepartureAirportId=a2.Id and a2.City=@Destination
JOIN Travel.Airport a3 ON a3.Id=f2.ArrivalAirportId and a3.City=@Origin and 
CAST(f2.DepartureTime AS DATE)= DATEADD(DAY, @NightInBeijing + DATEDIFF(DAY, f1.DepartureTime, f1.ArrivalTime), @FlightDate)
and f2.DepartureAirportId=f1.ArrivalAirportId
```


```sql
/*
    There is schema Travel with tables: Airport, Flight, and Company.
    Find the first flights from @CurrentTime from @Origin to declared @Destinations.
*/
DECLARE @Today DATE = GETDATE()
DECLARE @CurrentTime DATETIME = DATEADD(HOUR, 19, CONVERT(DATETIME, @Today))
DECLARE @Origin NVARCHAR(100) = 'London'
DECLARE @Destinations TABLE (City NVARCHAR(100))

INSERT INTO @Destinations (City)
VALUES ('Madrid'), ('Frankfurt'), ('Dubai'), ('Tokyo')


SELECT [Date],
Flight,
Departure, 
Arrival, 
Airline
FROM (
    SELECT
        [Date] = CONVERT(DATE,f.DepartureTime),
        Flight = UPPER(@Origin) + ' -> ' + UPPER(d.City),
        Departure = FORMAT(f.DepartureTime, 'HH:mm'),
        Arrival = FORMAT(f.ArrivalTime, 'HH:mm'),
        Airline = c.Name,
        ROW_NUMBER() OVER (PARTITION BY d.City ORDER BY f.DepartureTime ASC) AS Position
    FROM Travel.Flight f
    JOIN Travel.Airport a1 ON a1.Id = f.DepartureAirportId
    JOIN Travel.Airport a2 ON a2.Id = f.ArrivalAirportId
    JOIN @Destinations d ON d.City = a2.City
    JOIN Travel.Company c ON c.Id = f.CompanyId
    WHERE a1.City = @Origin AND f.DepartureTime >= @CurrentTime
) AS FlightList
WHERE Position < 2
ORDER BY [Date], Departure;
```

```sql
/*
    There is schema Travel with tables: Airport, Flight, and Company.
    Select the rank of 10 longest flights from the @Country.
*/
DECLARE @Country NVARCHAR(100) = 'Singapore'


/* C1
SELECT
	RankNumber = RANK() OVER (ORDER BY x.FlightTimeInMinute DESC)
   ,x.Flight
   ,FlightTime = FORMAT(x.FlightTimeInMinute / 60, '00') + ':' +
				 FORMAT(x.FlightTimeInMinute % 60, '00')
FROM
(
	SELECT DISTINCT TOP 10
		Flight = departure.City + ' -> ' + arrival.City
	   ,FlightTimeInMinute = DATEDIFF(MINUTE, f.DepartureTime, f.ArrivalTime)
	FROM Travel.Airport departure
	JOIN Travel.Flight f ON f.DepartureAirportId = departure.Id
	JOIN Travel.Airport arrival ON arrival.Id = f.ArrivalAirportId
	JOIN Travel.Company c ON c.Id = f.CompanyId
	WHERE departure.Country = @Country
	ORDER BY FlightTimeInMinute DESC
) x
*/

// C2
SELECT TOP 10 RankNumber = Rank() OVER (ORDER BY DIFF DESC), Flight, FlightTime =  CONCAT( FORMAT(DIFF / 60, '00'), ':', FORMAT(DIFF % 60, '00'))  FROM (
    SELECT 
        DIFF = DATEDIFF(MINUTE, f.DepartureTime, f.ArrivalTime),
        Position = ROW_NUMBER() OVER (PARTITION BY a.City, a2.City, DATEDIFF(MINUTE, f.DepartureTime, f.ArrivalTime) ORDER BY a.City, a2.City ASC),
        FlightDeparture = f.DepartureTime,
        Flight = a.City + ' -> ' + a2.City
        FROM Travel.Flight f 
        JOIN Travel.Airport a ON a.Id = f.DepartureAirportId 
        JOIN Travel.Airport a2 ON a2.Id = f.ArrivalAirportId
        WHERE a.Country = @Country
) as FlightTable
WHERE Position < 2
ORDER BY RankNumber, Flight
```


```sql
/*
    There is schema Travel with tables: Airport, Flight, and Company.
    Find connecting flights from @Origin to @Destination.
    @Date is a date when you want to leave the @Origin.
    @MinStopoverHours is the minimal time to make a transfer to connecting flight.
    @MaxStopoverHours is the maximal time that you want to spend at the airport.
*/
DECLARE @Origin NVARCHAR(100) = 'New York'
DECLARE @Destination NVARCHAR(100) = 'Beijing'
DECLARE @Date DATE = DATEADD(DAY, 1, GETDATE())
DECLARE @MinStopoverHours INT = 1
DECLARE @MaxStopoverHours INT = 10

SELECT
	Flight = origin.City + ' (' + FORMAT(flight.DepartureTime, 'HH:mm') + ') -> ' + 
		     stopover.City + ' (' + FORMAT(flight.ArrivalTime, 'HH:mm') + ')' 
   ,ConnectingFlight = stopover.City + ' (' + FORMAT(connectingFlight.DepartureTime, 'HH:mm') + ') -> ' + 
					   destination.City + ' (' + FORMAT(connectingFlight.ArrivalTime, 'HH:mm') + ')'
   ,[Time] = FORMAT(DATEDIFF(MINUTE, flight.DepartureTime, connectingFlight.ArrivalTime) / 60, '00') + ':' +
			 FORMAT(DATEDIFF(MINUTE, flight.DepartureTime, connectingFlight.ArrivalTime) % 60, '00')
FROM Travel.Airport origin
JOIN Travel.Flight flight ON flight.DepartureAirportId = origin.Id
JOIN Travel.Airport stopover ON stopover.Id = flight.ArrivalAirportId
JOIN Travel.Flight connectingFlight ON connectingFlight.DepartureAirportId = stopover.Id
							       AND connectingFlight.DepartureTime >= DATEADD(HOUR, @MinStopoverHours, flight.ArrivalTime)
								   AND connectingFlight.DepartureTime <= DATEADD(HOUR, @MaxStopoverHours, flight.ArrivalTime)
JOIN Travel.Airport destination ON destination.Id = connectingFlight.ArrivalAirportId
							   AND destination.City = @Destination
WHERE origin.City = @Origin
  AND CONVERT(DATE, flight.DepartureTime) = @Date
```