---
title: Theory
tags:
  - Database
---

###### tags: `Database`

# Theory
# ACID
## Atomicity
## Consistency
- Dữ liệu được bảo toàn khi sử dụng transaction

:::danger
- Giả có 2 bảng: like và pic => bảng like có id pic nhưng không có id nào tham chiếu trong bảng pic
- TH gửi $100 từ account 1 -> account 2. Trong quá trình gửi DB bị lỗi, account 1 bị trừ $100 nhưng
:::

## Isolation
- 
## Durability
- (Durability): đảm bảo rằng hiệu ứng của các giao dịch đã được xác nhận sẽ tồn tại vĩnh viễn, ngay cả trong trường hợp xảy ra lỗi, bao gồm cả các sự cố
