---
title: SQL
tags: [Database]

---

###### tags: `Database`
# SQL

# Sakila
#
<h4>Other</h4>

- SQL Wildcards: tìm kí tự đại diện
- SQL Aliases: đặt tên tạm thời cho table

<h4>Tutorial</h4>

- `SELECT`:  select data from a database.
- Syntax: (* chọn tất cả dữ liệu)
    >SELECT column1, column2, ...
FROM table_name;
- `SELECT DISTINCT` :  return only distinct (different) values. 
>SELECT DISTINCT column1, column2, ...
FROM table_name;
---
- `WHERE`: để lọc bản ghi (giống câu lệnh if)
> SELECT column1, column2, ...
FROM table_name
WHERE condition;

>Có thêm một số toán tử:
>`<>`: thay cho !=
> `BETWEEN`: tìm kiếm một phạm vi (BETWEEN ... AND ...)
> `LIKE`: tìm kiếm một mẫu (e.g bắt đầu bằng một kí tự nào đó)
>  `IN`: đa điều kiện (IN ('', '', ...)), shorthand của `OR` 
---> Có thể thêm NOT vào trước toán tử,  e.g NOT IN
----
- `AND` , `OR` , `NOT`
> SELECT column1, column2, ...
FROM table_name
WHERE condition1 OR condition2 OR condition3 ...;
---
- `ORDER BY`: giống với sort
> SELECT column1, column2, ...
FROM table_name
ORDER BY column1, column2, ... ASC|DESC;    
&emsp;---> Sắp xếp theo culumn1, nếu có hai giá trị bằng nhau thì sắp xếp theo column2 (DESC: sắp xếp giảm dần)
---
- `INSERT INTO`: chèn
>INSERT INTO table_name (column1, column2, column3, ...)
VALUES (value1, value2, value3, ...);
---
- `NULL`: kiểm tra một trường có giá trị không
>SELECT column_names
FROM table_name
WHERE column_name IS NULL;   &emsp;&emsp;(hoặc IS NOT NULL)
---
- `UPDATE`: sửa đổi bản ghi, nên có `WHERE` nếu không nó sẽ sửa đổi cả bảng
>UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
---
- `DELETE`: xóa một bản ghi tồn tại trong bảng
>DELETE FROM table_name WHERE condition; (xóa cả bảng thì không có WHERE)
---
- `SELECT TOP`: trả về số bản ghi từ trên xuống (e.g bảng có 12 bảng ghi, SELECT TOP 5 thì chỉ trả về 5 bản ghi đầu tiên)
>SELECT TOP number|percent column_name(s)
FROM table_name
WHERE condition;
---
- `MIN()` , `MAX()`: hàm trả về giá trị nhỏ nhất và lớn nhất của một column
>SELECT MIN(column_name)
FROM table_name
WHERE condition;

- `COUNT()`: số lượng cột thỏa mãn điều kiện
- `AVG()`: trả về trung bình cộng của cột
- `SUM()`: (syntax tương tự, thay tên hàm)
>SELECT COUNT(column_name)
FROM table_name
WHERE condition;
---
- `LIKE`: sử dụng trong `WHERE` để mẫu chỉ định
>SELECT column1, column2, ...
FROM table_name
WHERE columnN LIKE pattern;

![](https://scontent.fhan5-8.fna.fbcdn.net/v/t1.15752-9/272243218_1276506299539579_3209648952189621990_n.png?_nc_cat=108&ccb=1-5&_nc_sid=ae9488&_nc_ohc=Dhs6PCJnnYsAX-TPXdB&_nc_ht=scontent.fhan5-8.fna&oh=03_AVIrHU6C5AVhgT0LDrj1ADr5xOfYlUCHV6T-aBGReCnPTw&oe=623000C5)

<h3>SQL IN Operator</h3>

- The IN operator allows you to specify multiple values in a WHERE clause.
- The IN operator is a shorthand for multiple OR conditions.
>SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1, value2, ...);

or
>SELECT column_name(s)
FROM table_name
WHERE column_name IN (SELECT STATEMENT);

<h3>SQL Joins</h3>

- Sử dụng để kết hợp hàng từ nhiều bảng dựa trên cột có liên quan
![](https://i.imgur.com/IogiHVF.png)

- `INNER JOIN`:
>SELECT column_name(s)
FROM table1
INNER JOIN table2
ON table1.column_name = table2.column_name;
![](https://www.w3schools.com/sql/img_innerjoin.gif)

- `LEFT JOIN`:

![](https://www.w3schools.com/sql/img_leftjoin.gif)
![](https://www.w3schools.com/sql/img_rightjoin.gif)
![](https://www.w3schools.com/sql/img_fulljoin.gif)

<h3>SQL UNION Operator</h3>

- Phối hợp nhiều câu lênh `SELECT` với nhau
    - Every SELECT statement within UNION must have the same number of columns
    - The columns must also have similar data types
    - The columns in every SELECT statement must also be in the same order
- The UNION operator selects only distinct values by default. To allow duplicate values, use UNION ALL:

<h3>SQL GROUP BY Statement</h3>



<h3>SQL HAVING Clause</h3>

- Tương tự như WHERE nhưng có thể sử dụng được với các function COUNT

<h3>SQL EXISTS Operator</h3>

- Trả về sự truy vấn có sự tồn tại của bản ghi không
>SELECT column_name(s)
FROM table_name
WHERE EXISTS
(SELECT column_name FROM table_name WHERE condition);

<h3>SQL ANY and ALL Operators</h3>

- ANY: trả về true nếu ít nhất một truy vấn con thỏa mãn điều kiện

>SELECT column_name(s)
FROM table_name
WHERE column_name operator ANY
  (SELECT column_name
  FROM table_name
  WHERE condition);
  
- ALL: trả về true nếu tất cả truy vấn con thỏa mãn điều kiện

>SELECT column_name(s)
FROM table_name
WHERE column_name operator ALL
  (SELECT column_name
  FROM table_name
  WHERE condition);


<h3>SQL SELECT INTO Statement</h3>

- Copy giá trị từ một bảng sang một bảng mới
>SELECT column1, column2, column3, ...
INTO newtable [IN externaldb]
FROM oldtable
WHERE condition;

<h3>SQL INSERT INTO SELECT Statement</h3>

- Copy giá trị từ một mảng và chèn nó vào một bảng khác
>INSERT INTO table2 (column1, column2, column3, ...)
SELECT column1, column2, column3, ...
FROM table1
WHERE condition;

<h3>SQL CASE</h3>

- Giống với if-else
>CASE
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    WHEN conditionN THEN resultN
    ELSE result
END;
---
---
<h2>SQL Database</h2>

- ``CREATE DATABASE`` tạo một SQL database mới
>CREATE DATABASE databasename;
- ``DROP DATABASE``: xóa một SQL database
>DROP DATABASE databasename;
- `BACKUP DATABASE`: sao lưu dữ liệu hiện có
>BACKUP DATABASE databasename
TO DISK = 'filepath';

>BACKUP DATABASE testDB
TO DISK = 'D:\backups\testDB.bak'
WITH DIFFERENTIAL;
---> Sao lưu khi có sự khác biệt giữa bản cũ và mới

- `CREATE TABLE`: tạo một mảng mới trong một database
>CREATE TABLE table_name (
    column1 datatype,
    column2 datatype,
    column3 datatype,
   ....
);

- `DROP TABLE`: xóa một bảng trong SQL database
>DROP TABLE table_name;

<h3>SQL ALTER TABLE</h3>

- `ALTER TABLE`: thêm, xóa, biến đổi một cột bên trong một bảng đã tồn tại
- ADD: thêm    
>ALTER TABLE table_name
ADD column_name datatype;
- DROP: xóa
>ALTER TABLE table_name
DROP COLUMN column_name;
- ALTER TABLE - ALTER/MODIFY COLUMN: thay đổi dữ liệu của một cột trong bảng
>ALTER TABLE table_name
ALTER COLUMN column_name datatype;
---> SQL Sever

>ALTER TABLE table_name
MODIFY COLUMN column_name datatype;
---> My SQL

<h3>SQL Constraints</h3>

- chỉ định quy tắc cho dữ liệu trong bảng khi sử dụng CREATE TABLE hoặc  ALTER TABLE
>CREATE TABLE table_name (
    column1 datatype constraint,
    column2 datatype constraint,
    column3 datatype constraint,
    ....
);

- constraint có thể là:
    + NOT NULL: đảm bảo cột không có giá trị Null
    + UNIQUE: các giá trị trong cột là khác nhau
    + PRIMARY KEY: kết hợp của NOT NULL và UNIQUE
    + FOREIGN KEY: ngăn chặn hành động phá hủy liên kết giữa các bảng
    + CHECK: đảm bảo các giá trị trong mảng thỏa mãn điều kiện
    + CREATE INDEX: tạo chỉ số mảng


<h3>SQL CREATE INDEX</h3>

- Tạo chỉ số trong mảng
- `CREATE INDEX`: cho phép các chỉ số trùng lặp
>CREATE INDEX index_name
ON table_name (column1, column2, ...);
- `CREATE UNIQUE INDEX`: các chỉ số là duy nhất

- `DROP INDEX`: xóa một chỉ số mảng


<h3>SQL Date Type</h3>

- String data

![](https://scontent.fhan5-8.fna.fbcdn.net/v/t1.15752-9/248075729_1155528718550094_7523482613137824312_n.png?_nc_cat=110&ccb=1-5&_nc_sid=ae9488&_nc_ohc=y7DCjsE2uI8AX-LlDw0&_nc_ht=scontent.fhan5-8.fna&oh=03_AVL01Gfk9qiz7DU6ulRyrA_rL_O2UcLauNneOGvAf2xCug&oe=623154BE)

---
- Numberic

![](https://scontent.xx.fbcdn.net/v/t1.15752-9/p403x403/272050452_5387189614648751_2208889772925386246_n.png?_nc_cat=104&ccb=1-5&_nc_sid=aee45a&_nc_ohc=5bE7F937bXIAX_a2Tf-&_nc_ad=z-m&_nc_cid=0&_nc_ht=scontent.xx&oh=03_AVJo0pTdLOTJTPVq-1jKVozclQhzSwO8H-w8dBbV6uywSg&oe=62347743)

---