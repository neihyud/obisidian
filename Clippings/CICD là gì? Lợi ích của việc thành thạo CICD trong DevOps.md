---
title: CI/CD là gì? Lợi ích của việc thành thạo CI/CD trong DevOps
source: https://200lab.io/blog/ci-cd-la-gi/
author: []
published: 2024-10-07
created: 2024-12-03
description: CI/CD là một tập hợp các phương pháp và công cụ giúp tự động hóa quy trình phát triển, kiểm thử, và triển khai phần mềm
tags:
  - clippings
---
## 1\. CI/CD là gì?

CI/CD (Continuous Integration/Continuous Delivery - Deployment) là một tập hợp các phương pháp và công cụ giúp tự động hóa quy trình phát triển, kiểm thử, và triển khai phần mềm. Mục tiêu chính của CI/CD là đẩy nhanh tốc độ phát hành phần mềm, giảm lỗi và nâng cao tính ổn định của các phiên bản phát hành (release).

![](https://statics.cdn.200lab.io/2024/10/ci-cd-la-gi-ci-cd-1.png)

## 2\. CI là gì?

### 2.1 Định nghĩa CI

**CI** (Continuous Integration) là quá trình liên tục kiểm tra và tích hợp code mới vào code chính của một dự án. Mục tiêu của CI là đảm bảo rằng bất kỳ thay đổi nào trong code (chẳng hạn như sửa lỗi hoặc thêm tính năng) đều được kiểm tra ngay lập tức để phát hiện lỗi trước khi nó gây ảnh hưởng xấu đến hệ thống.

![](https://statics.cdn.200lab.io/2024/10/ci-cd-la-gi-ci-1.png)

CI hoạt động như một hệ thống kiểm tra tự động. Mỗi khi bạn thay đổi code và commit lên hệ thống, các bài kiểm tra tự động sẽ được chạy để kiểm tra xem code có hoạt động đúng và không gây lỗi hay không.

### 2.2 Những bước cơ bản trong CI

1. **Functional tests**: Kiểm tra xem phần mềm có hoạt động đúng theo mong đợi hay không. Đây thường là các bài kiểm tra tự động mà đội ngũ QA (Quality Assurance) phát triển để đảm bảo tính năng hoạt động đúng. VD: Người dùng có thể thêm sản phẩm vào giỏ hàng và thực hiện thanh toán thành công hay không.
2. **Security scans**: Kiểm tra code có lỗ hổng bảo mật nào không, chẳng hạn như khả năng bị SQL Injection hay [XSS](https://200lab.io/blog/giai-phap-bao-mat-giao-dien-web-ngan-chan-tan-cong-xss-va-csrf-hieu-qua/) (Cross-Site Scripting).
3. **Code quality scans**: Đảm bảo code tuân thủ các tiêu chuẩn, ví dụ như độ dài hàm, cách sử dụng khoảng trắng, và các quy tắc coding style. VD: Tiêu chuẩn như đặt tên biến.
4. **Performance tests**: Kiểm tra xem code có đáp ứng được yêu cầu về hiệu suất không, chẳng hạn như thời gian xử lý yêu cầu hoặc khả năng chịu tải.
5. **License scanning**: Kiểm tra xem tất cả các thư viện hoặc công cụ bạn sử dụng có giấy phép phù hợp hay không, để tránh các vấn đề pháp lý.
6. **Fuzz testing**: Gửi các dữ liệu bất thường, như chuỗi ký tự quá dài hoặc số không hợp lệ, vào ứng dụng để kiểm tra xem nó có bị crash hay gặp lỗi bất ngờ không.

### 2.3 Lợi ích của việc thiết lập CI

CI giúp phát hiện sớm các lỗi tiềm ẩn, tiết kiệm rất nhiều thời gian và công sức, rõ ràng việc sửa một lỗi ngay khi vừa phát hiện sẽ dễ dàng hơn nhiều so với khi lỗi đó đã tích hợp vào nhiều phần khác của hệ thống.

Ví dụ: Giả sử bạn đang phát triển một tính năng mới cho ứng dụng. Thay vì đợi đến cuối quá trình phát triển để kiểm tra, CI cho phép bạn thực hiện các bài kiểm tra ngay khi bạn viết code. Nhờ đó, nếu có lỗi xuất hiện, bạn có thể phát hiện ngay lập tức và sửa chữa khi vấn đề còn nhỏ, thay vì đợi đến khi tính năng đã hoàn thành và **tích hợp** với các phần khác.

![](https://statics.cdn.200lab.io/2024/10/ci-cd-la-gi-devops.jpg)

CI giúp toàn bộ đội nhóm đều có thể thấy tình trạng hiện tại của phần mềm, tất cả các thành viên từ lập trình viên, quản lý, đến QA (kiểm thử chất lượng), UX (trải nghiệm người dùng), và thậm chí cả bộ phận bảo mật có thể theo dõi tiến trình và trạng thái của dự án. Từ đó, mọi người có thể điều chỉnh công việc của mình để phù hợp với tình trạng thực tế.

Ví dụ: Khi các bài kiểm tra bảo mật được thực hiện thường xuyên, cả nhóm sẽ nhận biết được dự án có gặp phải các vấn đề bảo mật mới không. Nếu CI phát hiện rằng một đoạn code mới làm lộ lỗ hổng bảo mật, thì không chỉ các lập trình viên mà cả các quản lý dự án cũng biết ngay lập tức. Nhóm sẽ có thể phân bổ nguồn lực để sửa lỗi hoặc điều chỉnh kế hoạch phát triển.

## 3\. CD là gì?

### 3.1 Định nghĩa CD

**CD** là quá trình đưa code từ nơi bạn viết lên một nơi mà người dùng có thể sử dụng được, như website hoặc ứng dụng di động. Có 2 loại CD chính:

- **Continuous Delivery** (Phân phối liên tục): Đảm bảo rằng code luôn sẵn sàng để được triển khai bất cứ lúc nào. Sau khi code đã vượt qua các kiểm tra CI, nó được đặt ở trạng thái sẵn sàng để triển khai. Tuy nhiên, việc triển khai có thể được thực hiện thủ công.
- **Continuous Deployment** (Triển khai liên tục): Tự động triển khai code ngay khi nó vượt qua tất cả các kiểm tra. Không cần sự can thiệp thủ công, hệ thống sẽ tự động đưa code vào môi trường production.

![](https://statics.cdn.200lab.io/2024/10/ci-cd-la-gi-cd.png)

### 3.2 Những bước cơ bản trong CD

- **Build**: Trước khi triển khai code, đôi khi chúng ta cần phải thực hiện quá trình build cho các ngôn ngữ biên dịch (compiled language) như C, C++, Java, ...
- **Deploy**: Triển khai code: đẩy Docker image lên repository, dùng dòng lệnh để đưa code lên AWS, ... Triển khai có thể được tự động hoặc thủ công, tùy thuộc vào cách bạn thiết lập.

Ví dụ: Khi bạn hoàn thành thêm tính năng mới như giỏ hàng, CD sẽ tự động kiểm tra và đóng gói tính năng này rồi triển khai lên website thật để khách hàng có thể sử dụng ngay lập tức.

### 3.3 Lợi ích của việc thiết lập CD

Mục tiêu của CD là giúp việc triển khai và phát hành (release) phần mềm trở nên dễ dàng và có thể diễn ra thường xuyên mà không gặp nhiều rủi ro. Khi CD được thiết lập, mỗi lần bạn commit code, code có thể tự động được triển khai vào các môi trường như: review environment, Staging environment hay production environment.

Các môi trường khác ngoài production (như review và staging) hoạt động giống như những phép thử, nhằm đảm bảo code chạy ổn định trước khi release chính thức cho người dùng.

## 4\. Lợi ích của việc thành thạo CI/CD trong DevOps

- **Tăng tốc độ phát triển và triển khai phần mềm**: Việc tự động hóa quá trình kiểm tra và triển khai giúp các nhóm DevOps nhanh chóng đưa code vào môi trường production mà không làm gián đoạn hệ thống. Cho phép doanh nghiệp đưa các tính năng mới và bản vá lỗi ra thị trường nhanh chóng, đáp ứng nhu cầu của người dùng kịp thời.
- **Giảm thiểu lỗi và rủi ro trong quá trình phát triển**: CI/CD giúp giảm thiểu lỗi bằng cách tự động kiểm tra code sau mỗi lần commit. Nếu có vấn đề xuất hiện, nó sẽ được phát hiện sớm và sửa chữa trước khi triển khai lên production.
- **Tăng cường sự hợp tác trong nhóm**: Tất cả mọi người, từ developer đến tester, đều có thể thấy tiến độ của các bản build, giúp việc phối hợp tốt hơn nhằm đảm bảo phần mềm đạt chất lượng cao nhất.
- **Đảm bảo tính nhất quán**: CI/CD giúp duy trì tính nhất quán khi phát hành phần mềm bằng cách đảm bảo rằng tất cả các bản build đều tuân thủ quy trình kiểm tra chuẩn đã được thiết lập sẵn. Điều này giúp chất lượng phần mềm luôn ổn định.
- **Hỗ trợ mở rộng và duy trì hệ thống phức tạp**: Trong các hệ thống phần mềm lớn, việc mở rộng và duy trì hệ thống có thể trở nên rất phức tạp. CI/CD đóng vai trò quan trọng trong việc giảm tải và tự động hóa các quy trình này, đảm bảo sự ổn định và khả năng mở rộng của hệ thống.
- **Tối ưu hóa chi phí và tài nguyên**: CI/CD giúp tự động hóa các quy trình phát triển, kiểm thử và triển khai, từ đó tiết kiệm tài nguyên, thời gian từ đó tiết kiệm chi phí nhân lực và giảm lỗi phát sinh từ thao tác thủ công.
