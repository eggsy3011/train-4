Nginx / Apache 

Tìm hiểu Nginx là gì

Tìm hiểu Apache là gì

HTTP return code

200

301, 307

400, 401, 403, 404, 406, 408, 422

500, 502, 504

Bài tập 1: 

Trình bày lại các vấn đề đã tìm hiểu về apache, nginx, http return code tại: <name>/hosting-webserver/nginx-apache/http-return-code.md

Cài đặt LEMP / LAMP / reverse proxy


Cài đặt LEMP bao gồm các bước sau:

yum install epel-release

rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm

rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm

![image](https://github.com/eggsy3011/train-4/assets/108015833/5c4ac41f-0400-46fd-9702-06a830125eb1)

2. Cài đặt NGINX

yum install -y nginx

![image](https://github.com/eggsy3011/train-4/assets/108015833/f11d435b-38fa-4d4b-acdb-aa9b5ff22eb3)


3. Cài đặt PHP và Module

yum-config-manager --enable remi-php73

yum install -y php php-fpm php-common

yum install -y php-pecl-apcu php-cli php-pear php-pdo php-mysqlnd php-pecl-memcache php-pecl-memcached php-mbstring

![image](https://github.com/eggsy3011/train-4/assets/108015833/627f4b24-1b35-4362-a5dc-652f7af0e71b)


4. Cài đặt MariaDB #

yum install -y mariadb mariadb-serverCopied!
![image](https://github.com/eggsy3011/train-4/assets/108015833/28fc8193-7f06-4689-a36f-0154181492c0)

Start MariaDB và bật khi khởi động:

systemctl start mariadb.service
systemctl enable mariadb.service

![image](https://github.com/eggsy3011/train-4/assets/108015833/f7c265ce-373c-457a-b384-0f291d0bf7dc)


Cài đặt ban đầu và cấu hình mật khẩu cho user root MariaDB

mysql_secure_installation

Thực hiện các câu hỏi tiếp theo khi chạy lệnh này:

Enter current password for root (enter for none): Enter
Set root password? [Y/n] y
New password:
Re-enter new password:
Password updated successfully!
Remove anonymous users? [Y/n] y
Disallow root login remotely? [Y/n] y
Remove test database and access to it? [Y/n] y
Reload privilege tables now? [Y/n] y
Thanks for using MariaDB!

![image](https://github.com/eggsy3011/train-4/assets/108015833/7699c611-dbc4-4de8-ad65-cd26e81340c1)


5. Cấu hình NGINX #

Mở file config của NGINX để chỉnh sửa cấu hình:

nano /etc/nginx/nginx.conf
Chỉnh worker_processes bằng với số CPU trên server của bạn

![image](https://github.com/eggsy3011/train-4/assets/108015833/bf9fd1a8-44ca-4f48-b9b6-2a843e285f7d)


Cấu hình nginx virtual hosts

nano /etc/nginx/conf.d/default.conf

Sửa lại nội dung file như sau:

############

server {

listen       80 default_server;

server_name example.com;

 

location / {

root   /var/www/html;

index index.php index.html index.htm;

try_files $uri $uri/ /index.php?q=$uri&$args;

}

 

error_page  404     /404.html;

location = /404.html {

root   /usr/share/nginx/html;

}

 

error_page   500 502 503 504  /50x.html;

location = /50x.html {

root   /usr/share/nginx/html;

}

# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000

#

location ~ \.php$ {

root           /var/www/html;

fastcgi_pass 127.0.0.1:9000;

fastcgi_index index.php;

fastcgi_param  SCRIPT_FILENAME   $document_root$fastcgi_script_name;

include        fastcgi_params;

}

}

############

Lưu lại và restart dịch vụ để áp dụng cấu hình.

![image](https://github.com/eggsy3011/train-4/assets/108015833/8754ee0b-befe-4a9a-8d57-39c69ba08930)


systemctl stop nginx.service

systemctl start nginx.service

![image](https://github.com/eggsy3011/train-4/assets/108015833/24e1aed1-6c41-426d-bd2d-952474ff924e)


6. Cấu hình PHP-FPM #

Chỉnh sửa user và group

nano /etc/php-fpm.d/www.conf
user = nginx
group = nginx

![image](https://github.com/eggsy3011/train-4/assets/108015833/0e676491-9738-4424-b42d-a54c903f716f)

Restart dịch vụ php-fpm

systemctl stop php-fpm.service
systemctl start php-fpm.service
![image](https://github.com/eggsy3011/train-4/assets/108015833/9bc165ec-ca10-4c4f-a168-c034a5b86186)

7. Cấu hình firewalld #

firewall-cmd --permanent --add-port=80/tcp
firewall-cmd --permanent --add-port=443/tcp
firewall-cmd –reload
8. Kiểm tra hoạt động #

Chạy một file info.php để kiểm tra có Web Server có hoạt động hay không

nano /var/www/html/info.php

Thêm đoạn sau và lưu lại:

<?php
phpinfo();
?>
![image](https://github.com/eggsy3011/train-4/assets/108015833/6d2cc326-9cc7-47dc-934f-4468f43a4ef6)

![image](https://github.com/eggsy3011/train-4/assets/108015833/005a010c-e3ed-4013-8a28-fe935e3ded53)

Wordpress:
cd /tmp
wget https://wordpress.org/latest.tar.gz

![image](https://github.com/eggsy3011/train-4/assets/108015833/db56b337-69b0-4b5c-aef4-22cbc3b4c7f1)

tar -xzf latest.tar.gz
sudo mv wordpress/* /var/www/your_domain
![image](https://github.com/eggsy3011/train-4/assets/108015833/93ff5e69-d169-4628-8542-6b550d6c7638)

![image](https://github.com/eggsy3011/train-4/assets/108015833/ca125776-9d1c-4179-9100-5847b4aa79d1)

Cấp quyền thư mục:

sudo chown -R nginx:nginx /var/www/your_domain
sudo chmod -R 755 /var/www/your_domain

![image](https://github.com/eggsy3011/train-4/assets/108015833/c63b728c-308c-4d67-abb8-7a17bf6c98d7)
Cấu hình WordPress:
Tạo file cấu hình WordPress:

sudo cp /var/www/your_domain/wp-config-sample.php /var/www/your_domain/wp-config.php
sudo vi /var/www/your_domain/wp-config.php
![image](https://github.com/eggsy3011/train-4/assets/108015833/4304a67f-e0c0-4ad6-a174-4bb61669fd52)

 ![image](https://github.com/eggsy3011/train-4/assets/108015833/3364e376-c2c7-44e3-b64a-77345633d0fc)

 



Laravel

SSL

Bài tập 2: 

Sử dụng 1 vps cài đặt mô hình LAMP / LEMP / reverse proxy

Tạo 2 vhost: <name>.wordpress.zonecloud.co <name>.laravel.zonecloud.co

Source: wordpress -> <name>.wordpress.zonecloud.co laravel -> <name>.laravel.zonecloud.co

Setup SSL cho cả 2 domain

Reset passwd mysql user root

Mở remote mysql port 3306 cho phép bên ngoài truy cập

Thực hiện reset pass user admin (hoặc bất kì user nào bạn tạo lúc cài đặt wordpress).

Bài tập: Trình bày lại phần lab tại: <name>/hosting-webserver/reverse-proxy/wordpres-laravel.md
