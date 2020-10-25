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
```
Sau khi cài đặt xong, truy cập vào Acunetix qua địa chỉ https://127.0.0.1:13443

![ubuntu20 04-2020-10-25-15-21-12](https://user-images.githubusercontent.com/32956424/97102148-cb532100-16d5-11eb-979a-4447d5106574.png)


### Cài đặt Burp Suite

Burp Suite được viết bằng ngôn ngữ Java. Do đó, máy tính cần được cài đặt Java nếu muốn sử dụng Burp Suite

Nếu máy tính chưa được cài đặt Java, chạy lệnh sau

```
sudo apt install default-jre
sudo apt install default-jdk
```

Truy cập vào link http://portswigger.net/burp/download.html để download Burp Suite. Sau khi download, chạy file .JAR này để bắt đầu.



### 2.2. Demo SQL Injection

### 2.3. Demo Cross-site Scripting 
