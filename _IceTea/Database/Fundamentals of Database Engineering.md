- data được lưu dưới disk dưới dạng file, thuộc 1 hoặc nhiều block of data hoặc phân mảnh nằm rải rác 
- version mới (tuple)
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
	- **Dirty Read** là hiện tượng khi một giao dịch đọc dữ liệu chưa được commit từ một giao dịch khác. Cấp độ Repeatable Read đảm bảo rằng một giao dịch không đọc dữ liệu chưa được commit từ giao dịch khác.
		- đọc một cái gì đó mà transacion chưa commit
			- ex: trong tx1 có read giá trị của A nhưng chưa commit, nhưng trong thời gian đó tx2 thực hiện làm thay đổi giá trị của A, nếu ở dưới tx1 đọc lại A thì giá trị không còn giống như lúc ban đầu
		![[db-dirty-read.png]]
	- **non-repeatable reads (Đọc không lặp lại )**: xảy ra khi một transaction đọc dữ liệu, và sau đó dữ liệu này bị thay đổi bởi một transaction khác trước khi transaction đầu tiên hoàn thành. Cấp độ Repeatable Read đảm bảo rằng các dữ liệu được đọc bởi một giao dịch sẽ không thay đổi trong suốt thời gian giao dịch đó.
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

##### Shared Lock
- khi read data, data sẽ không bị thay đổi từ lúc đọc đến lúc kết thúc
- vẫn có thể read được data
#### Exclusive lock
- khi write data,  không có bất kỳ ai có thể xen vào quá trình ( không read, write)
- không cho phép bất kì **exclusive lock** hay **shared lock** nào khác được apply trên data đã có **exclusive lock**.

## Vacuum
- Clear **dead tuple**
- Cập nhập statistic information để tối ưu quá trình **query plan**
- Giải quyết vấn đề **over transaction id**
- 
