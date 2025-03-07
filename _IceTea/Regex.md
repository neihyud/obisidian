---
title: Regex
tags: [Programming]

---

# Regex
### Flags
- g (global): tìm tất cả các đoạn ký tự khớp
- m (multi line) khi có ^ và $ thì ^ sẽ là bắt đầu 1 dòng và $ là kết thúc 1 dòng trong chuỗi, nếu không có cờ m thì ^ và $ là bắt đầu và kết thúc của cả chuỗi
- i (insensitive) không phân biệt chữ hoa chữ thường(vd:aBc sẽ khớp với pattern /abc/i )


### Toán tử
- `[xyz]`: trùng khớp với 1 kí tự trong [ ]
- `[0-9]`: trùng khớp với 1 kí tự từ 0 - 9
- `[^0-9]`: không trùng khớp với tất cả các ký tự trong []
- `x|y`: x hoặc y
----------------------
### Vị trí 
- `^`: trùng khớp với phần đấu 
- `$`: trùng khớp với p cuối của chuỗi
    - c$ (chữ c cuối cùng)
- `^\w+`: trùng khớp với chữ đầu tiên
- `+` <=> `{1,}`: trùng khớp với 1 hoặc nhiều lần ký tự đứng trước nó
    - \d+: trùng với một con số
    - c+: c or cc...
---------------------
### Số lượng
- `*` <=> `{0,}`:  trùng khớp với 0 hoặc nhiều ký tự đứng trước nó 
- `?`<=> `{0,1}`: trùng khớp với 0 hoặc 1 lần ký tự đứng trước nó
- `.`: trùng với 1 ký tự đơn bất kỳ ngoại trừ ngắt dòng
- `x{n}`: trùng khớp đúng với n lần ký tự đứng trước nó. n >= 0. 
    - \d{2} sẽ bắt đc các số có 2 chữ số đứng liền nhau.
- `x{n,}`: trùng khớp với ít nhất n lần ký tự đứng trước nó
    - \d{2,}: >2 số đứng liền nhau
- `x{n,m}`: khớp >n, <m
---------------------
### Phân loại
- `\b`: toàn bộ ký tự đứng trước nó
    - hello\b: hello world, invalid: helloworld
    - \b: k có kí tự nào đứng trước nó
- `\B`: một phần ký tự đứng trước nó, ngược với \b
- `\d`: 1 ký tự số từ 0-9 
    - '\d+': tìm chuỗi số 
- `\s`: 1 khoảng trắng bao gồm tab và xuống dòng
- `\S`: 1 ký tự k p khoảng trắng
- `\w`: khớp với tất cả kí tự là chữ, số và gạch dưới. Tương đương với mẫu [A-Za-z0-9_].
- `\W`: k p là từ
    - \W: k khớp % trong 100%
- `uxxxx`: ký tự unicode
    - \u00FA: 'ú'
    - \u00F9: 'ù'
- `\pL`: unicode bất kì ngoài trừ dấu cách
- `\`: bỏ qua không coi đó là cú pháp
                        
### Other
- (x): group có nhớ x
- (?:x): khớp x n k nhớ x
- x(?=y): x theo sau là y, chỉ xét x
- x(?!y): x theo sau k là y
