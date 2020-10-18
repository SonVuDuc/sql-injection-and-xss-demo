# Demo SQL Injection và Cross-site Scripting 
## 1. Giới thiệu về SQL Injection và Cross-site Scripting 
### 1.1. SQL Injection

SQL Injection là một kỹ thuật lợi dụng những lỗ hổng kiểm tra dữ liệu đầu vào của các ứng dụng web. Kỹ thuật này được thực hiện bằng cách chèn thêm một đoạn SQL để làm sai lệnh đi câu truy vấn ban đầu, từ đó có thể khai thác dữ liệu từ database. SQL injection có thể cho phép hacker thực hiện các thao tác như một người quản trị web, trên cơ sở dữ liệu của ứng dụng.

Ví dụ trong form đăng nhập của trang web, thay vì nhập username và password thông thường, hacker sẽ chèn thêm những câu lệnh SQL bất hợp pháp để lấy dữ liệu của người dùng hoặc dữ liệu database.

SQL Injectiton có thể gây ra:
  + Hack tài khoản cá nhân
  + Đánh cắp, sao chép dữ liệu
  + Thay đổi cấu trúc hoặc xóa cơ sở dữ liệu hệ thống
  + Hacker có thể đăng nhập với tư cách người dùng khác, thậm chí là quản trị viên
  + Đánh cắp thông tin cá nhân người dùng 


### 1.2. Cross-site Scripting 
## 2. Demo
### 2.1. Chuẩn bị

  + Hệ điều hành: Ubuntu 20.04 Desktop
  + XAMPP (Tải về và cài đặt tại https://www.apachefriends.org/download.html)
  + Github

### 2.2. Demo SQL Injection

Tiến hành cài đặt XAMPP để tạo web server và cơ sở dữ liệu. Start các dịch vụ của XAMPP

![Screenshot from 2020-10-18 16-16-27](https://user-images.githubusercontent.com/32956424/96363269-63dd2480-115d-11eb-958f-98a5f3352dc5.png)

Truy cập vào địa chỉ http://localhost để kiểm tra XAMPP đã hoạt động hay chưa

![Screenshot from 2020-10-18 16-16-48](https://user-images.githubusercontent.com/32956424/96363304-9e46c180-115d-11eb-90f7-24aac52a01aa.png)

Truy cập vào địa chỉ http://localhost/phpmyadmin/ để kiếm tra MySQL

![Screenshot from 2020-10-18 16-20-46](https://user-images.githubusercontent.com/32956424/96363349-e7971100-115d-11eb-8b18-a9a2307c9092.png)

Tiến hành pull web mẫu trên github bằng lệnh 

```
git clone https://github.com/FrancescoBorzi/sql-injection-demo.git
```
Truy cập vào trang chủ thông qua địa chỉ http://localhost/sql-injection-demo/index.php

![Screenshot from 2020-10-18 16-26-15](https://user-images.githubusercontent.com/32956424/96363446-ab17e500-115e-11eb-9625-53e14ee45320.png)

Đăng nhập thông qua mục **Standard Login**, chọn phần **Vulnerable**

![Screenshot from 2020-10-18 16-28-02](https://user-images.githubusercontent.com/32956424/96363487-ed412680-115e-11eb-86a0-392b5a69db13.png)

Trang web mẫu sẽ hiện ra truy vấn MySQL để tiện cho việc xem xét SQL Injection

![Screenshot from 2020-10-18 16-28-02](https://user-images.githubusercontent.com/32956424/96363487-ed412680-115e-11eb-86a0-392b5a69db13.png)

#### Case 1: Đăng nhập với username mà không cần password

Nếu hacker biết được username của một user, có thể là người dùng bình thường hoặc quản trị viên. Với lỗ hổng SQL Injection, hacker có thể dễ dàng đăng nhập với username đã biết mà không cần password.

Ví dụ username đã biết ở đây là **admin**. Hacker sẽ truyền vào chuỗi ```' OR '1'='1'; -- ```

![Screenshot from 2020-10-18 16-32-49](https://user-images.githubusercontent.com/32956424/96363590-96881c80-115f-11eb-9b4a-8820c34ed12d.png)

Hacker đã đăng nhập thành công với tư cách là admin. Trong mục **Query Executed** thấy rằng truy vấn đã thực hiện việc truy vấn. Điều kiện password là ```''``` hoặc ```'1'='1'```. Dễ dàng nhận thấy điều kiện ```'1'='1'``` luôn đúng. Ngoài ra toán tử ```--``` ở cuối có tác dụng khiến hệ thống bỏ qua mọi thứ đằng sau dấu ```--```. Khiến hacker dễ dàng đăng nhập với username đã biết

#### Case 2: Đăng nhập mà không biết username và password

Thông thường hacker sẽ sử dụng tool quét lỗ hổng SQL Injection trên nhiều trang web khác nhau. Khi đã xác định được trang web có lỗ hổng. Hacker sẽ bắt đầu khai thác lỗ hổng

Trong trường hợp hacker không biết trước username như **case 1**. Hacker sẽ sử dụng truy vấn ```' OR '1'='1'; -- ```

![Screenshot from 2020-10-18 16-42-37](https://user-images.githubusercontent.com/32956424/96363856-f92de800-1160-11eb-8b74-0f5684581f30.png)

Đăng nhập thành công với username **' or '1'='1'; -- **

![Screenshot from 2020-10-18 16-41-14](https://user-images.githubusercontent.com/32956424/96363827-ca177680-1160-11eb-84c2-97f98c64cd20.png)

#### Case 3: Đăng nhập và xóa Database

Lỗ hổng SQL Injection còn cho hacker thực thi những truy vấn theo ý muốn











### 2.3. Demo Cross-site Scripting 
