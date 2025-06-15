---
layout: post
title:  "John the Ripper - công cụ crack mật khẩu trên Linux"
author: hungbeo197
categories: [ tech, security ]
image: assets/images/2025/2.JohntheRipper/JohntheRipper.png
comments: false
---
John the Ripper, nghe rất quen thuộc. Nếu bạn là một tín đồ tìm hiểu về những câu chuyện trinh thám, hay tìm hiểu hàng loạt về những vụ án kinh dị từ thời xưa thì chắc hẳn bạn cũng biết Jack the Ripper. 

Mình cũng không rõ Jack the Ripper nhiều, nhưng mình cũng tìm hiểu qua một chút. Jack the Ripper là một kẻ giết người hàng loạt không rõ danh tính. Hắn hành động vào năm 1888, ở những khu vực có đa phần người nghèo sinh sống xung quanh khu vực Whitechapel, London, Anh. Whitechapel là nơi có nhiều phụ nữ hành nghề mại dâm. Mỗi lần Jack hành nghề, hắn luôn rạch cổ họng, cắt xẻ phần bụng, phanh thây xác nạn nhân.

Vậy John the Ripper có liên quan gì về chức năng hay sự kiện gì với nhau không? KHÔNG. Nhưng 2 tên gọi trên có mối liên hệ ở tên gọi mang tính ẩn dụ.

John the Ripper là một công cụ mã nguồn mở dùng để crack mật khẩu, được phát triển bởi Openwall. Công cụ này để crack mật khẩu với mục đích tốt như pentesting, kiểm thử độ mạnh của mật khẩu. John the Ripper mang tính ẩn dụ, ngụ ý rằng nó "xé toạc", "phanh thây" các mật khẩu yếu - tương tự như cách Jack xé xác nạn nhân.

Bài viết này mình không kể chuyện trinh thám, mình sẽ viết một chút chi tiết về cách sử dụng John the Ripper.

## John the Ripper là gì?
John the Ripper là một công cụ bẻ khóa mật khẩu hiệu quả. Chúng được sử dụng để kiểm tra độ mạnh mật khẩu, phát hiện mật khẩu yếu trong hệ thông, kiểm tra an ninh.

**Các định dạng được hỗ trợ**
* Hash cho các hệ thống Linux (Gồm /etc/shadow)
* Hash cho Windows LM/BTLM
* Các hash phổ biến như MD5, SHA-1, SHA-256

**Môi trường hoạt động**
* Chủ yếu là Linux, mặc dù có sẵn cho cả môi trường Windows, macOS.

## John the Ripper hoạt động như thế nào?
John dùng nhiều phương pháp để phá hash.
### 1. Từ điển tấn công (Dictionary Attack)
* Dùng danh sách mật khẩu (wordlist), hash từng mật khẩu, so sánh với hash mục tiêu.
* Ví dụ: nếu ```password123``` có trong danh sách và hash khớp thì crack thành công.
* Lệnh thực hiện:
  ```
  john --wordlist=/path/to/wordlist.txt hashes.txt
  ```

### 2. Tấn công dựa trên quy tắc (Rule-based Attack)
* Áp dụng quy tắc lên từ điển (Chuyển chữ hoa, thêm số, thêm ký tự đặc biệt).
* Ví dụ: ```password``` -> ```password1``` -> ```P@ssword123```, ...
* Lệnh thực hiện:
  ```
  john --wordlist=/path/to/wordlist.txt --rules hashes.txt
  ```

### 3. Tấn công duyệt trâu (Brute Force Attack)
* Thử mọi kết hợp của các ký tự.
* Hiệu quả cho hash ngắn, nhưng quét mật khẩu rất chậm
* Lệnh thực hiện:
  ```
  john --mask='?a?a?a?a' hashes.txt
  ```

### 4. Thử tăng dần (Increamental Mode)
* Là phiên bản tối ưu của Brute Force, sử dụng thuật toán riêng để thử theo thứ tự hiệu quả nhất.
* Lệnh thực hiện:
  ```
  john --incremental hashes.txt
  ```

## Cách sử dụng cơ bản
### 1. Chuẩn bị file hash
Tạo file ```hashes.txt```, ví dụ:
```
5f4dcc3b5aa765d61d8327deb882cf99
```
(Đây là MD5 của "password")

### 2. Chuẩn bị wordlist (danh sách mật khẩu)
Trong John the Ripper, "wordlist" đóng vai trò rất quan trọng trong việc crack mật khẩu. Nó là tập tin văn bản chứa danh sách các mật khẩu thường gặp hoặc rò rỉ trong quá khứ. Wordlist giúp tăng hiệu quả khi phân tích các giá trị hash.

#### Tải wordlist RockYou
Một trong những wordlist nổi tiếng nhất là "RockYou" - được tạo ra từ cơ sử dữ liệu mật khẩu bị rò rỉ và hiện là danh sách tiêu chuẩn trong nhiều công cụ hacking.

1. Bạn có thể tải RockYou từ Github.
* [URL RockYou](https://github.com/danielmiessler/SecLists "RockYou")
* Vị trí RockYou thường là:
  ```
  SecLists/Passwords/Leaked-Databases/rockyou.txt.tar.gz
  ```

2. Sau khi tải về, bạn giải nén bằng lệnh:
  ```
  tar -xzf rockyou.txt.tar.gz
  ```

3. Giải nén xong sẽ được file ```rockyou.txt```, chứa hàng triệu mật khẩu phổ biến.

#### Tầm quan trọng và lưu ý khi dùng wordlist
* Tầm quan trọng: Wordlist càng chất lượng thì khả năng crack thành công càng cao. RockYou là ví dụ thực tế, nhưng nếu biết thông tin cụ thể về mục tiêu (như tên công ty, sinh nhật, v.v.) thì nên tạo wordlist tùy chỉnh.
* Lưu ý: Do wordlist có thể rất lớn, nên sẽ ảnh hưởng đến tốc độ xử lý và bộ nhớ RAM trong quá trình chạy.

### 3. Chạy John the Ripper
Ví dụ:
#### 1. Tấn công từ điển (dictionary attack)
```
john --format=raw-md5 --wordlist=/path/to/rockyou.txt hashes.txt
```

* --format=raw-md5: chỉ định loại hash (ở đây là MD5)
* --wordlist=...: chỉ định đường dẫn tới wordlist
* hashes.txt: chứa hash bạn muốn crack

#### 2. Hiển thị kết quả đã giải mã
```
john --show --format=raw-md5 hashes.txt
```

## Ứng dụng thực tế của John the Ripper
### 1. Kiểm tra bảo mật
Quản trị viên hệ thống có thể dùng John để phân tích hash mật khẩu được lưu, từ đó phát hiện và yêu cầu người dùng thay đổi những mật khẩu yếu.

### 2. Đào tạo, giáo dục về an toàn mạng
John là công cụ lý tưởng để thực hành trong các khóa học về bảo mật, giúp học viên thấy rõ nguy cơ thực tế khi dùng mật khẩu yếu.

Tuyệt đối không được sử dụng cho mục đích xấu.

## Giới hạn của John the Ripper và những biện pháp đối phó 
### 1. Thuật toán tốn tài nguyên tính toán
John the Ripper không hiệu quả khi gặp các thuật toán bảo mật cau như: bcrypt, Argon2. Chúng được thiết kế để tính toán chậm, vì vậy rất khó crack.

### 2. Đối phó với những mật khẩu mạnh
Các mật khẩu phức tạp và dài cũng rất khó bị phá bằng bất kì kĩ thuật thông thường nào. Vì vậy, vẫn chưa có một biện pháp đối phó với những mật khẩu mạnh.

Tuy nhiên, bạn có thể dựa vào điểm yếu này để tạo mật khẩu cho bạn. Mật khẩu cho bạn cần có:
* Thuật toán hash phù hợp với nhu cầu bảo mật
* Mật khẩu dài và đa dạng

## Tóm lại
* John the Ripper là công cụ mạnh mẽ để kiểm thử độ mạnh của mật khẩu.
* Cần sử dụng **ĐÚNG MỤC ĐÍCH, KHÔNG ĐƯỢC SỬ DỤNG TRÁI PHÉP**.
* Tìm hiểu kỹ cách John the Ripper là bước đầu để xây dựng hệ thống an toàn hơn. 