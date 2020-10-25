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
  + Acunetix
  + Sqlmap
  + Burp Suite

### Cài đặt XAMPP
Tiến hành cài đặt XAMPP để tạo web server và cơ sở dữ liệu. Start các dịch vụ của XAMPP

![Screenshot from 2020-10-18 16-16-27](https://user-images.githubusercontent.com/32956424/96363269-63dd2480-115d-11eb-958f-98a5f3352dc5.png)

Truy cập vào địa chỉ http://localhost để kiểm tra XAMPP đã hoạt động hay chưa

![Screenshot from 2020-10-18 16-16-48](https://user-images.githubusercontent.com/32956424/96363304-9e46c180-115d-11eb-90f7-24aac52a01aa.png)

Truy cập vào địa chỉ http://localhost/phpmyadmin/ để kiếm tra MySQL

![Screenshot from 2020-10-18 16-20-46](https://user-images.githubusercontent.com/32956424/96363349-e7971100-115d-11eb-8b18-a9a2307c9092.png)

### Cài đặt Acunetix

Pull repo bằng lệnh

```
git clone https://github.com/neolead/acunetix-linux.git
```
Login với quyền **root**, chạy lệnh sau để cài Acunetix:

```
apt install unzip curl ; rm -rf /tmp/acun*
apt install libxdamage1 libgtk-3-0 libasound2 libnss3 libxss1 libx11-xcb1 -y
cd /tmp; rm master.zip -f
curl -L -o master.zip http://github.com/neolead/acunetix-linux/zipball/master/
unzip master.zip
cd `ls|grep neolead` && cat acupatch* > acupatch.tgz
tar -zxvf acupatch.tgz
chmod +x ./acunetix_trial.sh
./acunetix_trial.sh
service acunetix_trial stop
chmod 0755 patch_awvs
cp patch_awvs /home/acunetix/.acunetix_trial/v_190515149/scanner/
su - acunetix -c "/home/acunetix/.acunetix_trial/v_190515149/scanner/patch_awvs"
cd /tmp;rm -rf /tmp/acu*
service acunetix_trial stop
chattr +i /home/acunetix/.acunetix_trial/data/license/license_info.json
service acunetix_trial start
curl -k https://127.0.0.1:13443
su - acunetix -c "/home/acunetix/.acunetix_trial/ && bash change_credentials.sh"
```
Sau khi cài đặt xong, truy cập vào Acunetix qua địa chỉ https://127.0.0.1:13443

![ubuntu20 04-2020-10-25-15-24-43](https://user-images.githubusercontent.com/32956424/97102211-40265b00-16d6-11eb-8cb0-1a093ffe2cdc.png)



### Cài đặt Burp Suite

Burp Suite được viết bằng ngôn ngữ Java. Do đó, máy tính cần được cài đặt Java nếu muốn sử dụng Burp Suite

Nếu máy tính chưa được cài đặt Java, chạy lệnh sau

```
sudo apt install default-jre
sudo apt install default-jdk
```

Truy cập vào link http://portswigger.net/burp/download.html để download Burp Suite. Sau khi download, chạy file .JAR này để bắt đầu.

![ubuntu20 04-2020-10-25-15-23-17](https://user-images.githubusercontent.com/32956424/97102218-503e3a80-16d6-11eb-8cd4-6025ae60c7f9.png)

Chọn next để tiếp tục 

![ubuntu20 04-2020-10-25-15-23-42](https://user-images.githubusercontent.com/32956424/97102226-5f24ed00-16d6-11eb-86b0-d4bc1c998f2d.png)


Chọn next để tiếp tục 

![ubuntu20 04-2020-10-25-15-23-46](https://user-images.githubusercontent.com/32956424/97102231-6a781880-16d6-11eb-9fd5-40faab0dc669.png)

Giao diện của Burp Suite

![ubuntu20 04-2020-10-25-15-24-04](https://user-images.githubusercontent.com/32956424/97102238-7368ea00-16d6-11eb-83f1-8db17190d5f8.png)

Burp được thiết kế để sử dụng cùng với trình duyệt. Hoạt động giống như một HTTP proxy server, tất cả HTTP và HTTPS traffic đều sẽ đi qua Burp. Trước khi tiến hành làm việc với Burp, cần cấu hình để trình duyệt để làm việc với Burp 

Tại giao diện của Burp Suite, chọn **Proxy** -> **Option**. Trong phần **Proxy Listener**, kiểm tra xem checkbox Running đã được chọn hay chưa, và Interface là 127.0.0.1:8080. Nếu các thông tin không đúng như trên, Add 1 proxy với IP và Port là 127.0.0.1:8080

![ubuntu20 04-2020-10-25-15-39-16](https://user-images.githubusercontent.com/32956424/97102457-474e6880-16d8-11eb-84b3-932dc27703dd.png)

Cần cấu hình trình duyệt để thay đổi proxy. Click vào **Preference** -> **Network Setting**

![ubuntu20 04-2020-10-25-15-43-14](https://user-images.githubusercontent.com/32956424/97102530-d8254400-16d8-11eb-9cbb-578db11aff22.png)

Chọn **Manual Proxy Configuration** và điền IP **127.0.0.1** và Port **8080**

![ubuntu20 04-2020-10-25-15-43-18](https://user-images.githubusercontent.com/32956424/97102564-21759380-16d9-11eb-8e4d-028305e0ac6d.png)


### Cài đặt SQL MAP

Cần cài đặt Python trước để có thể sử dụng sqlmap

```
sudo apt-get install -y sqlmap
```

### 2.2. Demo SQL Injection

Cài đặt website demo của OWASP

```
docker run -d -p 80:80 -p 443:443 --name owasp17 bltsec/mutillidae-docker
```
Truy cập vào trang chủ qua địa chỉ http://127.0.0.1/mutillidae/ 

![ubuntu20 04-2020-10-25-15-55-02](https://user-images.githubusercontent.com/32956424/97102727-76fe7000-16da-11eb-81a9-a130dc34b3ae.png)

Truy cập vào trang login của Mutillidae

![ubuntu20 04-2020-10-25-15-56-52](https://user-images.githubusercontent.com/32956424/97102779-e5dbc900-16da-11eb-97cb-b2678dba3160.png)

Sử dụng Acunetix để scan các lỗ hổng. Bằng cách tạo 1 target để scan, nhập URL trang login của Mutillidae

![ubuntu20 04-2020-10-25-15-57-01](https://user-images.githubusercontent.com/32956424/97102809-20456600-16db-11eb-8ad2-f0faa21a9b64.png)

Click vào nút **Scan**

![ubuntu20 04-2020-10-25-15-57-10](https://user-images.githubusercontent.com/32956424/97102817-2d625500-16db-11eb-84b7-d1cd443d3486.png)

Tại đây có thể tùy chọn Full scan hoặc Scan một lỗ hổng cụ thể

![ubuntu20 04-2020-10-25-15-57-15](https://user-images.githubusercontent.com/32956424/97102825-3e12cb00-16db-11eb-9c4c-9986816dbf90.png)

Quá trình scan sẽ cho thấy mức độ threat 

![ubuntu20 04-2020-10-25-15-57-43](https://user-images.githubusercontent.com/32956424/97102830-4965f680-16db-11eb-8310-0f90ecff8e13.png)

Như vậy là Website có lỗ hổng SQL Injection để khai thác. Thử nhập chuỗi ```' OR 1=1; #``` vào ô username

![ubuntu20 04-2020-10-25-16-03-16](https://user-images.githubusercontent.com/32956424/97102891-c5f8d500-16db-11eb-8e02-434d9b6c0c21.png)

Click View Account Details. Như vậy là đã lấy được toàn bộ thông tin user trong bảng accounts của database

![ubuntu20 04-2020-10-25-16-03-31](https://user-images.githubusercontent.com/32956424/97102893-cc874c80-16db-11eb-973b-26693d0f4e7c.png)

Chuyển sang trang ```mutillidae/index.php?page=sqlmap-targets.php``` , vào mục View Someones Blog

![ubuntu20 04-2020-10-25-16-06-46](https://user-images.githubusercontent.com/32956424/97102975-62bb7280-16dc-11eb-9f44-6794ee1220cd.png)

Trong mục này người dùng có thể xem được blog của bất kỳ ai

![ubuntu20 04-2020-10-25-16-06-53](https://user-images.githubusercontent.com/32956424/97102982-74047f00-16dc-11eb-8874-e4fec3b7f9eb.png)

Trong Burp Suite, chuyển **Intercept** từ **off** sang **on** để có thể bắt các dữ liệu

![ubuntu20 04-2020-10-25-16-07-00](https://user-images.githubusercontent.com/32956424/97102991-88e11280-16dc-11eb-90fe-2cbfd95a2a7e.png)

Khi một người click vào xem blog của bất kỳ ai đó, thì Burp Suite sẽ chặn lại request đó lại, người sử dụng Burp Suite có thể xem, chỉnh sửa request

![ubuntu20 04-2020-10-25-16-07-34](https://user-images.githubusercontent.com/32956424/97103059-e1b0ab00-16dc-11eb-9c56-b349d7d2760d.png)

Tiến hành copy toàn bộ request và paste vào một file txt

![ubuntu20 04-2020-10-25-16-07-54](https://user-images.githubusercontent.com/32956424/97103073-eecd9a00-16dc-11eb-9840-7dfb1d6d7587.png)

![ubuntu20 04-2020-10-25-16-08-24](https://user-images.githubusercontent.com/32956424/97103081-f55c1180-16dc-11eb-9f0f-1857379091d4.png)

Chạy SQLMap, với input là file sample.txt vừa tạo ở trên











### 2.3. Demo Cross-site Scripting 
