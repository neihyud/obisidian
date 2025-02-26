![[Pasted image 20250214100305.png]]
![[Pasted image 20250214100320.png]]
```
chown <user> <file> # changes the owner and/or group for the specified files
chmod <rights> <file> # changes access rights to files and directories
chgrp <group> <file> # allows users to change groups
```

# Cron
![[cron_job.png]]
# ACID Requirements
- `Atomicity` (nguyên tử): Guarantees that the transaction will be executed completely or not executed at all.
- `Consistency`(nhất quán): Ensures that each successful transaction captures only valid results (any inconsistencies are excluded).
- `Isolation` (độc lập): Guarantees that one transaction cannot affect the other in any way.
- `Durability`(bền vững): Guarantees that the changes made by the transaction are saved.
# Database
https://github.com/cheatsnake/backend-cheats?tab=readme-ov-file#mongodb

## Chuẩn hóa dữ liệu
- **1NF**:
	- mỗi cột chỉ chứa một giá trị duy nhất (không chứa danh sách, tập hợp dữ liệu)![[ex_1nf.png]]
- **2NF**:
	- đạt chuẩn 1NF
	- trong một table, những field không phải primary key phải phụ thuộc vào primary key
	 ![[ex_2nf.png]]
- **3NF**:
	- Một cột không khóa, không phụ thuộc vào cột không khóa khác
	

| Dạng chuẩn hóa | Điều kiện                                                                                |
| -------------- | ---------------------------------------------------------------------------------------- |
| **1NF**        | Một cột không chứa danh sách hoặc tập hợp giá trị trong một ô.                           |
| **2NF**        | Mọi cột không khóa trong một bảng phải phụ thuộc hoàn toàn vào khóa chính (Primary Key). |
| **3NF**        | Một cột không khóa không được phụ thuộc vào một cột không khóa khác.                     |
# Docker
![[docker_syntax.png]]
### Horizontal and vertical scaling
![[visual_scaling.png]]