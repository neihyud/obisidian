# Concurrency Control
- Chuẩn SQL định nghĩa **bốn cấp độ cô lập giao dịch**.
	- **Serializable**: Bất kỳ tập hợp giao dịch Serializable nào, khi thực thi đồng thời, đều phải tạo ra kết quả giống hệt như khi chạy từng giao dịch một cách tuần tự theo một thứ tự nào đó.
		- Ba cấp độ còn lại được xác định dựa trên các **hiện tượng bất thường** có thể xảy ra khi các giao dịch chạy đồng thời. Vì **Serializable đảm bảo kết quả như chạy tuần tự**, nên **nó ngăn chặn hoàn toàn các hiện tượng này**.
	- **Dirty Read (Đọc bẩn)**  
		🔸 Một transaction đọc dữ liệu do một transaction khác chưa commit ghi vào.  
		📌 **Vấn đề:** Nếu giao dịch kia bị rollback, dữ liệu đã đọc sẽ trở thành không hợp lệ.
	- **Nonrepeatable Read (Đọc không lặp lại được)**  
		🔸 Một transaction đọc lại dữ liệu mà nó đã đọc trước đó và thấy rằng dữ liệu đã bị một giao dịch khác sửa đổi và commit.  
		📌 **Vấn đề:** Kết quả của cùng một truy vấn lại thay đổi trong cùng một giao dịch.
	- **Phantom Read (Đọc bóng ma)**  
		🔸 Một transaction chạy lại một truy vấn (chẳng hạn `SELECT` với điều kiện nào đó) và phát hiện rằng tập kết quả đã thay đổi do một transaction khác đã commit (thêm, sửa, xóa dòng dữ liệu).  
		📌 **Vấn đề:** Tập dữ liệu không nhất quán trong quá trình giao dịch.
	- **Serialization Anomaly (Lỗi tuần tự hóa)**  
		🔸 Kết quả của việc commit một nhóm transaction không thể khớp với **bất kỳ thứ tự thực thi tuần tự nào** của các giao dịch đó.  
		📌 **Vấn đề:** Vi phạm tính toàn vẹn dữ liệu do thứ tự thực thi không thể xác định.

<strong>Table&nbsp;13.1.&nbsp;Transaction Isolation Levels</strong>
<table class="table" summary="Transaction Isolation Levels" border="1">
        <colgroup>
          <col>
          <col>
          <col>
          <col>
          <col>
        </colgroup>
        <thead>
          <tr>
            <th>Isolation Level</th>
            <th>Dirty Read</th>
            <th>Nonrepeatable Read</th>
            <th>Phantom Read</th>
            <th>Serialization Anomaly</th>
          </tr>
        </thead>
        <tbody>
          <tr>
            <td>Read uncommitted</td>
            <td>Allowed, but not in PG</td>
            <td>Possible</td>
            <td>Possible</td>
            <td>Possible</td>
          </tr>
          <tr>
            <td>Read committed</td>
            <td>Not possible</td>
            <td>Possible</td>
            <td>Possible</td>
            <td>Possible</td>
          </tr>
          <tr>
            <td>Repeatable read</td>
            <td>Not possible</td>
            <td>Not possible</td>
            <td>Allowed, but not in PG</td>
            <td>Possible</td>
          </tr>
          <tr>
            <td>Serializable</td>
            <td>Not possible</td>
            <td>Not possible</td>
            <td>Not possible</td>
            <td>Not possible</td>
          </tr>
        </tbody>
      </table>


## ### Read Committed Isolation Level
- Default isolation level in PostgreSQL
	- Một truy vấn **SELECT** (không có `FOR UPDATE/SHARE`) **chỉ thấy dữ liệu đã được commit trước khi truy vấn bắt đầu**.
	- Nó **không bao giờ thấy dữ liệu chưa commit** hoặc **các thay đổi được commit bởi các giao dịch đồng thời trong quá trình truy vấn đang chạy**.
	- Hiểu đơn giản, **mỗi truy vấn SELECT hoạt động trên một ảnh chụp (snapshot) của cơ sở dữ liệu tại thời điểm nó bắt đầu chạy**.
	- Tuy nhiên, **nó vẫn thấy các thay đổi được thực hiện trong cùng một giao dịch, ngay cả khi chưa commit**.
 **📌Lưu ý:** Hai lệnh `SELECT` liên tiếp **trong cùng một giao dịch có thể thấy dữ liệu khác nhau**, nếu có giao dịch khác commit thay đổi **giữa hai lệnh này**
 
 ### **Hành vi của UPDATE, DELETE, SELECT FOR UPDATE/SHARE**
- Các lệnh `UPDATE`, `DELETE`, `SELECT FOR UPDATE`, `SELECT FOR SHARE` **chỉ tìm thấy các dòng dữ liệu đã commit tại thời điểm lệnh bắt đầu chạy**.
- **Nhưng nếu dòng dữ liệu đó đã bị cập nhật (hoặc xóa hoặc khóa) bởi một giao dịch khác**, thì giao dịch hiện tại sẽ phải **chờ giao dịch trước đó commit hoặc rollback**.

✅ Nếu giao dịch đầu tiên **rollback**, thì giao dịch thứ hai **có thể tiếp tục cập nhật dòng dữ liệu ban đầu**.  
✅ Nếu giao dịch đầu tiên **commit**, thì:
- Nếu dòng dữ liệu bị xóa, giao dịch thứ hai **bỏ qua dòng đó**.
- Nếu dòng dữ liệu bị cập nhật, giao dịch thứ hai **kiểm tra lại điều kiện WHERE**, nếu vẫn phù hợp, nó **sẽ cập nhật phiên bản mới nhất của dòng dữ liệu**.

Với `SELECT FOR UPDATE` và `SELECT FOR SHARE`, **phiên bản cập nhật của dòng dữ liệu sẽ bị khóa và trả về cho client**.

### **Hành vi của INSERT với ON CONFLICT**

- `INSERT ... ON CONFLICT DO UPDATE` hoạt động tương tự như `UPDATE`:
    - Nếu có xung đột (conflict) với dòng dữ liệu từ một giao dịch khác chưa hiển thị với `INSERT`, thì `UPDATE` sẽ áp dụng trên dòng đó.
- `INSERT ... ON CONFLICT DO NOTHING` có thể **không chèn dữ liệu** nếu có giao dịch khác đã **commit thay đổi** làm cho điều kiện `ON CONFLICT` kích hoạt.
### **Hành vi của MERGE**
- Lệnh `MERGE` kết hợp **INSERT, UPDATE, DELETE**.
- `MERGE` có thể thực hiện **các hành động khác nhau tùy điều kiện**, nhưng:
    - Nếu **cùng một dòng dữ liệu bị cập nhật bởi giao dịch khác**, thì `MERGE` **sẽ sử dụng phiên bản cập nhật mới nhất**.
    - Nếu dòng dữ liệu bị xóa, `MERGE` sẽ kiểm tra các điều kiện `NOT MATCHED` và thực thi hành động phù hợp.
    - Nếu `MERGE` cố gắng `INSERT` và bị lỗi trùng lặp (do giao dịch khác cũng chèn dòng đó), nó **sẽ báo lỗi thay vì xử lý lại điều kiện MATCHED**.
### **Nhược điểm của Read Committed**
- **Giao dịch có thể thấy ảnh chụp không nhất quán của dữ liệu**:
    - Nó có thể thấy **các thay đổi của một giao dịch khác trên cùng một dòng**,
    - Nhưng **không thấy các thay đổi trên các dòng khác mà giao dịch đó thực hiện**.

### Example
### **Tóm lại: Khi nào nên dùng Read Committed?**
✅ **Ưu điểm**:
- Nhanh và dễ sử dụng.
- Đủ tốt cho **các truy vấn đơn giản**.
🛑 **Nhược điểm**:

- **Không phù hợp với các truy vấn phức tạp**, nơi cần **nhất quán dữ liệu chặt chẽ hơn**.
- Các lệnh `UPDATE` hoặc `DELETE` có thể **bỏ sót dữ liệu** do cách PostgreSQL xử lý ảnh chụp dữ liệu mới mỗi lần chạy lệnh.

🔹 **Nếu ứng dụng cần truy vấn phức tạp và đảm bảo dữ liệu nhất quán**, bạn nên cân nhắc **Repeatable Read hoặc Serializable** thay vì Read Committed.

## Repeatable Read
- Cấp độ cô lập **Repeatable Read** chỉ thấy dữ liệu đã được **commit trước khi giao dịch bắt đầu**. Nó **không bao giờ thấy dữ liệu chưa commit** hoặc **các thay đổi được commit bởi các giao dịch đồng thời trong khi giao dịch đang chạy**.

📌 **Tuy nhiên**, mỗi truy vấn bên trong giao dịch **vẫn thấy các thay đổi trước đó được thực hiện trong cùng một giao dịch**, ngay cả khi chúng chưa commit.

💡 Đây là một mức đảm bảo mạnh hơn so với yêu cầu của tiêu chuẩn SQL cho cấp độ này. Nó ngăn chặn **tất cả các hiện tượng bất thường** được mô tả trong **Bảng 13.1**, **ngoại trừ lỗi bất đồng bộ tuần tự (serialization anomalies)**. Điều này hoàn toàn **được phép** theo tiêu chuẩn SQL, vì tiêu chuẩn chỉ quy định mức bảo vệ tối thiểu cho mỗi cấp độ cô lập.
### **Sự khác biệt giữa Repeatable Read và Read Committed**

🚀 **Repeatable Read** khác với **Read Committed** ở điểm:

- Trong **Read Committed**, mỗi truy vấn **thấy ảnh chụp (snapshot) tại thời điểm truy vấn bắt đầu**.
- Trong **Repeatable Read**, mọi truy vấn **đều thấy cùng một snapshot của cơ sở dữ liệu tại thời điểm câu lệnh đầu tiên của giao dịch được thực thi**.
- Vì vậy, **các lệnh SELECT liên tiếp trong cùng một giao dịch sẽ thấy cùng một dữ liệu**, bất kể có giao dịch khác commit thay đổi sau đó.

📌 Ứng dụng sử dụng cấp độ này **phải chuẩn bị để thử lại giao dịch nếu xảy ra lỗi tuần tự hóa (serialization failure).**
### **Hành vi của UPDATE, DELETE, MERGE, SELECT FOR UPDATE/SHARE**

- Các lệnh **UPDATE, DELETE, MERGE, SELECT FOR UPDATE, SELECT FOR SHARE** chỉ tìm thấy các dòng dữ liệu **đã được commit tại thời điểm giao dịch bắt đầu**.
- Nếu dòng đó đã bị cập nhật (hoặc xóa hoặc khóa) bởi một giao dịch khác trước khi lệnh được thực thi, giao dịch **Repeatable Read sẽ chờ giao dịch kia commit hoặc rollback**.

✅ Nếu giao dịch kia **rollback**, giao dịch **Repeatable Read có thể tiếp tục cập nhật dòng dữ liệu ban đầu**.  
🛑 Nếu giao dịch kia **commit**, giao dịch Repeatable Read **sẽ bị rollback ngay lập tức** với lỗi:
```
ERROR: could not serialize access due to concurrent update
```
💡 Lý do: Một giao dịch Repeatable Read không thể sửa đổi hoặc khóa dòng dữ liệu đã bị thay đổi sau khi giao dịch bắt đầu.

📌 Khi gặp lỗi này, ứng dụng cần hủy giao dịch và thử lại từ đầu.

Lần thử lại sau, giao dịch sẽ thấy phiên bản mới nhất của dòng dữ liệu và không còn xảy ra lỗi.
📢 Lưu ý: Chỉ các giao dịch cập nhật dữ liệu mới có thể bị lỗi này.

Giao dịch chỉ đọc (read-only transactions) không bao giờ bị lỗi tuần tự hóa.
### **Ưu điểm của Repeatable Read**

✅ **Đảm bảo rằng mỗi giao dịch thấy một phiên bản ổn định của cơ sở dữ liệu**, tránh được nhiều lỗi dữ liệu bị thay đổi trong khi giao dịch đang chạy.

🚀 Tuy nhiên, ảnh chụp này **không nhất thiết phải luôn nhất quán với một thứ tự thực thi tuần tự của tất cả các giao dịch đồng thời**.  
Ví dụ:

- Một giao dịch **chỉ đọc** có thể thấy một **bản ghi kiểm soát đã cập nhật rằng một lô dữ liệu đã hoàn thành**,
- Nhưng **không thấy một số bản ghi chi tiết của lô đó**, vì nó đọc một phiên bản cũ hơn của bản ghi kiểm soát.
- Vì vậy, nếu muốn áp dụng các quy tắc kinh doanh chính xác, cần **sử dụng khóa rõ ràng để ngăn chặn các giao dịch xung đột**.
### **Mối quan hệ với Snapshot Isolation**

📌 **Repeatable Read trong PostgreSQL được triển khai dựa trên kỹ thuật Snapshot Isolation (SI)**.

- Một số hệ thống cơ sở dữ liệu khác có thể cung cấp **Repeatable Read và Snapshot Isolation như hai cấp độ cô lập riêng biệt**.
- PostgreSQL kết hợp cả hai khái niệm, nhưng vẫn có một số khác biệt về **hiệu suất và hành vi** khi so sánh với các hệ thống dùng phương pháp khóa truyền thống.

💡 Các hiện tượng khác biệt giữa hai kỹ thuật này chỉ được nghiên cứu sau khi tiêu chuẩn SQL ra đời, nên **tiêu chuẩn SQL không mô tả chi tiết về Snapshot Isolation**. Nếu muốn tìm hiểu sâu hơn, bạn có thể tham khảo tài liệu **[berenson95]**.

### **Tóm tắt: Khi nào nên dùng Repeatable Read?**

✅ **Ưu điểm**

- **Ngăn chặn hầu hết các lỗi về tính nhất quán dữ liệu**, trừ lỗi bất đồng bộ tuần tự.
- **Giao dịch chỉ đọc sẽ luôn thấy một ảnh chụp ổn định của cơ sở dữ liệu**, giúp truy vấn dữ liệu chính xác hơn.

🛑 **Nhược điểm**

- **Cần xử lý lỗi rollback khi có giao dịch đồng thời cập nhật dữ liệu**.
- **Có thể thấy dữ liệu không đồng nhất** nếu không có khóa rõ ràng (ví dụ: thấy bản ghi kiểm soát đã cập nhật nhưng không thấy tất cả bản ghi chi tiết tương ứng).

🔹 **Khi nào nên dùng?**

- Khi **cần đảm bảo dữ liệu ổn định trong suốt giao dịch**, đặc biệt là với các giao dịch chỉ đọc.
- Khi **có thể chấp nhận thử lại giao dịch nếu gặp lỗi rollback do cập nhật đồng thời**.
- Khi **hệ thống không yêu cầu tính tuần tự tuyệt đối**, vì Repeatable Read không đảm bảo dữ liệu nhất quán với một lịch sử thực thi tuần tự.

🛑 **Khi nào không nên dùng?**

- Khi **cần đảm bảo không có lỗi bất đồng bộ tuần tự**, lúc này nên sử dụng **Serializable** thay vì Repeatable Read.
---

## Serializable
- Thực thi giao dịch nối tiếp đối với tất cả các transaction đã được cam kết, giống như thể các giao dịch được thực thi lần lượt từng giao dịch một thay vì đồng thời. Tuy nhiên, giống như mức **Repeatable Read**, các ứng dụng sử dụng mức này phải sẵn sàng thực hiện lại các transaction do lỗi tuần tự hóa. Thực tế, mức cô lập này hoạt động giống hệt **Repeatable Read**, nhưng nó cũng giám sát các điều kiện có thể làm cho một tập hợp các transaction có thể tuần tự hóa hoạt động theo cách không nhất quán với tất cả các thứ tự thực thi tuần tự có thể có của những giao dịch đó. Việc giám sát này không gây ra bất kỳ sự chặn nào ngoài những gì đã có trong **Repeatable Read**, nhưng nó có một số chi phí bổ sung. Nếu phát hiện điều kiện có thể gây ra lỗi tuần tự hóa, hệ thống sẽ kích hoạt lỗi tuần tự hóa.
### **Lưu ý khi sử dụng Serializable**
- **Không coi dữ liệu đọc là hợp lệ cho đến khi giao dịch cam kết thành công.**  
    Điều này áp dụng cho cả giao dịch chỉ đọc, trừ khi đó là giao dịch **READ ONLY DEFERRABLE** (loại này đảm bảo ảnh chụp dữ liệu ban đầu không có lỗi).
- **Khi nhận lỗi tuần tự hóa, phải thực hiện lại toàn bộ giao dịch.**  
    Khi một giao dịch bị hủy do lỗi tuần tự hóa, ứng dụng phải khởi động lại giao dịch từ đầu để có thể nhìn thấy thay đổi từ các giao dịch khác đã cam kết.
- **Giao dịch chỉ đọc sẽ không gặp lỗi tuần tự hóa.**  
    Chỉ các giao dịch thực hiện **UPDATE**, **DELETE**, **INSERT**, **MERGE** mới có thể bị hủy do lỗi tuần tự hóa.
### **Cách PostgreSQL đảm bảo Serializable**
PostgreSQL sử dụng **khóa dự đoán (predicate locking)** để theo dõi các phụ thuộc đọc/ghi giữa các giao dịch.  
Các khóa này:
- **Không gây chặn (blocking)** và không dẫn đến deadlock.
- **Được sử dụng để phát hiện lỗi tuần tự hóa** bằng cách theo dõi sự thay đổi của dữ liệu đọc trong các giao dịch khác.
- **Được hiển thị trong bảng hệ thống `pg_locks`** với chế độ khóa `SIReadLock`.
#### **Tối ưu hiệu suất khi sử dụng Serializable**
- **Khai báo giao dịch là `READ ONLY` khi có thể** để giảm thiểu khóa không cần thiết.
- **Giới hạn số lượng kết nối đang hoạt động** bằng cách sử dụng connection pool.
- **Chỉ bao gồm những thao tác cần thiết trong một transaction**, tránh kéo dài thời gian chạy không cần thiết.
- **Tránh giữ phiên giao dịch ở trạng thái "idle in transaction" quá lâu**, có thể sử dụng tham số `idle_in_transaction_session_timeout` để tự động đóng phiên giao dịch không hoạt động.
- **Loại bỏ khóa bảng và `SELECT FOR UPDATE`, `SELECT FOR SHARE`** nếu đã được bảo vệ bởi Serializable.
- **Tăng tham số `max_pred_locks_per_transaction`, `max_pred_locks_per_relation`, `max_pred_locks_per_page`** nếu hệ thống gặp nhiều lỗi tuần tự hóa do thiếu bộ nhớ khóa dự đoán.
- **Hạn chế sử dụng `SEQUENTIAL SCAN`**, vì nó yêu cầu khóa toàn bộ bảng, dễ gây lỗi tuần tự hóa. Cách khắc phục:
    - Giảm `random_page_cost`
    - Tăng `cpu_tuple_cost` để khuyến khích PostgreSQL sử dụng **chỉ mục (index scan)** thay vì quét toàn bộ bảng.
# Index

- chỉ hiệu quả trên những query có where condition trên column index
-  optimize queries that contain one or more `WHERE` or `JOIN` clauses of the form
	- _`indexed-column`_ : cột được đánh index
	- `indexable-operator`: là toán tử có thể sử dụng với index
	- `comparison-value`: là giá trị so sánh với cột
![[Pasted image 20250304091925.png]]
	- 
- khi sử dụng **B-Tree index**:
	- Không có tác dụng khi tìm kiếm text sử dụng điều kiện LIKE `% %` Index nữa index mãi thì tốc độ tìm kiếm.. không thay đổi mà tốc độ insert còn tăng lên.
	- với **compose index** , WHERE condition cần **match full value** hoặc **match mostleft column** để đạt hiệu quả tối đa.
- index giúp truy vấn nhanh hơn, nhưng nó cũng làm chậm update, insert, delete vì nó phải cập nhật lại index mỗi khi dữ liệu thay đổi
	- insert:
		- Thêm dữ liệu vào bảng chính (heap table).  
		- Cập nhật tất cả các index liên quan đến bảng đó.
	- update:
		- Ghi một bản ghi mới vào bảng (PostgreSQL sử dụng MVCC, không cập nhật trực tiếp mà tạo bản sao mới).
		- Cập nhật lại tất cả index có chứa cột bị thay đổi. (**xóa giá cũ khỏi index và thêm giá mới vào index**.)
	- delete:
		- Xóa bản ghi trong bảng.  
		- Xóa bản ghi tương ứng trong tất cả index liên quan.  
		- PostgreSQL không xóa dữ liệu ngay lập tức mà đánh dấu là "dead tuple" (bản ghi chết) -> cần VACUUM để dọn dẹp.
- Cập nhật các field không chứa index => không cập nhập index table ( kỹ thuật này gọi là  Heap-only Tuples ) (tránh cập nhập index khi nếu field chứa index không đổi)
- 
# Index Type
## B-tree
- By default, B-tree indexes store their entries in ascending order with nulls last
- The index can also be scanned backward, producing output satisfying `ORDER BY x DESC` (or more verbosely, `ORDER BY x DESC NULLS FIRST`, since `NULLS FIRST` is the default for `ORDER BY DESC`).
- You can adjust the ordering of a B-tree index by including the options `ASC`, `DESC`, `NULLS FIRST`, and/or `NULLS LAST` when creating the index; for example:
```
CREATE INDEX test2_info_nulls_low ON test2 (info NULLS FIRST);
CREATE INDEX test3_desc_index ON test3 (id DESC NULLS LAST); 
```
- B-trees can handle equality and range queries on data that can be sorted into some ordering. In particular, the PostgreSQL query planner will consider using a B-tree index whenever an indexed column is involved in a comparison using one of these operators:
```
	<   <=   =   >=   >  `BETWEEN` and `IN`  `IS NULL`  `IS NOT - NULL`
```
- cũng sử dụng với LIKE nếu điều kiên là hằng số hoặc bắt đầu bằng string: 'foo%' nhưng không bắt đầu băgnf '%foo'
## Hash
```
=
```

## Multi Index
- sẽ tìm kiếm theo điều keiẹn bên trái nhất để đạt hiệu quả
## Index on express
- chỉ sử dụng khi cần tốc độ truy vấn hơn là tốc độ update, insert
# Explain

```
EXPLAIN SELECT * FROM tenk1;
                         QUERY PLAN
-------------------------------------------------------------
 Seq Scan on tenk1  (cost=0.00..445.00 rows=10000 width=244)


- Estimated start-up cost. This is the time expended before the output phase can begin, e.g., time to do the sorting in a sort node.
    
- Estimated total cost. This is stated on the assumption that the plan node is run to completion, i.e., all available rows are retrieved. In practice a node's parent node might stop short of reading all available rows (see the `LIMIT` example below).
    
- Estimated number of rows output by this plan node. Again, the node is assumed to be run to completion.
    
- Estimated average width of rows output by this plan node (in bytes).
```

# Excute
## Index scan
- It is important to understand that an index speeds up data access at a certain maintenance cost. For each operation on indexed data, whether it be insertion, deletion, or update of table rows, indexes for that table need to be updated too, and in the same transaction
![[index_scan.png]]
- With index scanning, the access method returns TID values one by one until the last matching row is reached. The indexing engine accesses the table rows indicated by TIDs in turn, gets the row version, checks its visibility against multiversion concurrency rules, and returns the data obtained.
## Bitmap scan
![[bitmap_scan.png]]
The access method first returns all TIDs that match the condition (Bitmap Index Scan node), and the bitmap of row versions is built from these TIDs. Row versions are then read from the table (Bitmap Heap Scan), each page being read only once.

