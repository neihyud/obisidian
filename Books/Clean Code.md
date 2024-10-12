# Chapter 4: Comments
- Không nên viết comment => vì code luôn thay đổi. Sử dụng code rõ ràng
- giải thích nó trong code 
```
// Check to see if the employee is eligible for full benefits if ((employee.flags & HOURLY_FLAG) && (employee.age > 65))

=> if (employee.isEligibleForFullBenefits())
```
- comment tốt nhất là không comment
- comment: cung cấp thông tin hữu ích về cách triển khai và ý định đằng sau nó
- comment: cảnh báo hậu quả, đánh dấu to do (nên solve nó khi có thể)
tạo js docs

không nên comment thừa thãi
- không nên dùng comment khi có thể dùng hàm hoặc biến