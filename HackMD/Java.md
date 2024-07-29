---
title: Java
tags: [Programming]

---

###### tags: `Programming`

# Java
# Overview
![](https://i.imgur.com/Wx7Jotl.png)

![](https://i.imgur.com/M8JE058.png)

![](https://i.imgur.com/pvjagk2.png)

![](https://i.imgur.com/1oXoODy.png)

https://www.geeksforgeeks.org/difference-between-byte-code-and-machine-code/

- Một ứng dụng Java có thể có quyền truy cập ngay vào nhiều bộ nhớ hơn khi chúng tôi nâng cấp phần cứng nhưng Node chạy trên một luồng đơn.
- là ngôn ngữ biên dịch và thông dịch 
    - biên dịch: quá trình chuyển đổi mã nguồn thành mã máy tạo tệp thực thi mà máy tính có thể chạy trực tiếp, **thực thi kiểm tra lỗi ở thời điểm biên dịch**
    - thông dịch: là quá trình chuyển đổi mã nguồn thành mã trung gian (bytecode), **thực thi kiểm tra lỗi ở thời điểm thực thi** (chạy từng dòng, ít bộ nhớ hơn)
    - Quá trình thực thi
        - trước khi thực thi javac biên dịch source => bytecode (file .class)
        - khi chạy, JVM thông dịch và thực thi file .class
# Thuật ngữ
- **1. JVM (Java Virtual Machine)**: 3 giai đoạn thực hiện một chương trình
    - viết
    - compile (biên dịch): **JAVAC** compiler - chính trong JDK, input: chương trình, output: **bytecode**
    - run: JVM thực thi bytecode do biên dịch tạo ra
> mỗi hệ điều hành có JVM khác nhau, output sau khi thực thi bytecode giống nhau => ngôn ngữ độc lập nền tảng
- **2. ByteCode**: jvc chuyển java source => bytecode => jvm thực thi bytecode
    - lưu dưới dạng .class
    - javap: xem bytecode
    - phải sử dụng thông dịch => mã máy thì máy tính mới có thể đọc được
- **2.5 Machine Code**: (là binary - 0 1) máy có thể hiểu trực tiếp và được xử lý bởi CPU
- **3. JDK (Java Development Kit)**: bộ cung cụ phát triển Java bao gồm: compiler, JRE (Java Runtime Environment)
- **4. JRE (Java Runtime Environment**: JRE cho phép chương trình java chạy, không biên dịch nó, để chạy cần có JRE
- **5. Garbage Collector**: để xóa hoặc thu hồi bộ nhớ
- **6. ClassPath**: filepath nơi java runtime và java compiler tìm kiếm .class file để load
- 7. **JIT**: trình biên dịch giúp tối hóa khi biên dịch sang mã máy

# Ưu điểm
- là ngôn ngữ bậc cao
- **độc lập nền tảng**: chạy được trên bất kỳ nền tảng nào cài đạt JVM
- kiểu dữ liệu cụ thể
- cơ chế xử lý lỗi: try catch, ...
# Nhược điểm
- hiệu xuất
- quản lý bộ nhớ: quản lý bộ nhớ tự động có thể => hiệu suất chậm và tăng sử dụng bộ nhớ
# Các tính năng chính
- 1. **Độc lập với nền tảng**: mỗi hệ điều hành có JVM khác nhau, output sau khi thực thi bytecode giống nhau (bytecode chạy được trên nhiều nền tảng)
- 2. **Ngôn ngữ lập trình hướng đối tượng**: trừu tượng, đóng gói, kế thừa, đa hình
- 3. **Đơn giản**: k có tình năng như con trỏ, đa kế thừa, cấp phát bộ nhớ rõ ràng
- 4. **Mạnh mẽ**: thu gom rác, xử lý ngoại lệ và phân bổ bộ nhớ
- 5. **Đa luồng**: thực thi đồng thời nhiều p chương trình => tối đa CPU

- **Java độc lập nền tảng nhưng JVM thì phụ thuộc**
- String[] args: có thể nhập tham số vào java program ở command line
- static: 
- main: phương thức quan trọng nhất, nếu không có gây lỗi biên dịch (call JVM)

# Basics of Java
# Java Basic Syntax
## Basic terminologies in Java
1. Class: là một bản thiết kế của một instace (thực thể)
2. Object: là một thực thể của class, có hành động và trạng thái
3. Method: hành vi của một object
4.  Instance variables: thường tạo trạng thái của object 
## Systax
1. Source File Name
- Tên file giống với tên class, nếu không có public class thì tên có thể khác
2. Case Sensitivity
- có phân biệt chữ hoa, chữ thường
3. Class Names
- Ký tự đầu tiên viết Hoa
4. Method Names
- bắt đầu bằng lowercase (chữ thường)
5. Indentifiers trong java
- là một biến local, thực thể
6. Access Modifiers
- kiểm soát phạm vi của class và method
    - Access Modifiers: default, public, protected, private
    - Non-access Modifiers: final, final, abstract, static, transient, synchronized, volatile, native.






# Other
## Làm tròn số
- làm tròn chính xác

```javascript=
private static double round(double value, int places) {
    if (places < 0) throw new IllegalArgumentException();

    BigDecimal bd = new BigDecimal(Double.toString(value));
    // places: số số sau số thập phân
    bd = bd.setScale(places, RoundingMode.HALF_UP);
    return bd.doubleValue();
}
```