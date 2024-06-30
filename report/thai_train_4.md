Nginx / Apache 

# Tìm hiểu Nginx là gì:

Nginx (phát âm là "Engine-X") là một máy chủ web mã nguồn mở có hiệu suất cao được phát triển bởi Igor Sysoev

# Tìm hiểu Apache là gì:

Apache HTTP Server, thường gọi là Apache, là một trong những máy chủ web phổ biến nhất hiện nay. Apache được phát triển bởi Apache Software Foundation và là một phần của dự án mã nguồn mở.

# HTTP return code

# 200 :

Mã phản hồi này chỉ ra rằng yêu cầu của client đã thành công và máy chủ đã trả về tài nguyên yêu cầu.

# 301, 307

301: Mã này chỉ ra rằng tài nguyên yêu cầu đã được di chuyển vĩnh viễn đến URL mới và tất cả các yêu cầu trong tương lai nên sử dụng URL mới này.

307: Mã này chỉ ra rằng tài nguyên yêu cầu tạm thời được di chuyển đến URL mới. Client nên sử dụng URL gốc cho các yêu cầu trong tương lai.

# 400, 401, 403, 404, 406, 408, 422
400: Bad Request

Mã này chỉ ra rằng yêu cầu từ client có cú pháp không đúng hoặc không thể hiểu được bởi máy chủ.

401: Unauthorized

Mã này chỉ ra rằng yêu cầu cần phải xác thực. Client cần cung cấp thông tin đăng nhập hợp lệ.

403: Forbidden

Mã này chỉ ra rằng máy chủ đã hiểu yêu cầu nhưng từ chối thực hiện nó. Điều này thường do quyền truy cập bị hạn chế.

404: Not Found

Mã này chỉ ra rằng máy chủ không thể tìm thấy tài nguyên yêu cầu. URL yêu cầu không tồn tại trên máy chủ.

406: Not Acceptable

Mã này chỉ ra rằng máy chủ không thể tạo phản hồi phù hợp với các tiêu chí do client đặt ra trong tiêu đề Accept.

408: Request Timeout

Mã này chỉ ra rằng máy chủ không nhận được yêu cầu đầy đủ từ client trong khoảng thời gian quy định.

422: Unprocessable Entity

Mã này chỉ ra rằng máy chủ hiểu nội dung loại phương tiện của yêu cầu nhưng không thể xử lý nó do lỗi ngữ nghĩa.

# 500, 502, 504

500: Internal Server Error

Mã này chỉ ra rằng máy chủ gặp lỗi nội bộ và không thể hoàn thành yêu cầu của client.

502: Bad Gateway

Mã này chỉ ra rằng máy chủ, hoạt động như một gateway hoặc proxy, nhận được phản hồi không hợp lệ từ máy chủ ngược dòng.

504: Gateway Timeout

Mã này chỉ ra rằng máy chủ, hoạt động như một gateway hoặc proxy, không nhận được phản hồi kịp thời từ máy chủ ngược dòng.

Bài tập 1: 

Trình bày lại các vấn đề đã tìm hiểu về apache, nginx, http return code tại: <name>/hosting-webserver/nginx-apache/http-return-code.md

# Cài đặt LEMP / LAMP / reverse proxy


# Cài đặt LEMP bao gồm các bước sau:

```
yum install epel-release
```
```
rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm
```
```
rpm -Uvh http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm
```
![image](https://github.com/eggsy3011/train-4/assets/108015833/5c4ac41f-0400-46fd-9702-06a830125eb1)

# 2. Cài đặt NGINX
```
yum install -y nginx
```
![image](https://github.com/eggsy3011/train-4/assets/108015833/f11d435b-38fa-4d4b-acdb-aa9b5ff22eb3)

# 3. Cài đặt PHP và Module
```
yum-config-manager --enable remi-php73
```
```
yum install -y php php-fpm php-common
```
```
yum install -y php-pecl-apcu php-cli php-pear php-pdo php-mysqlnd php-pecl-memcache php-pecl-memcached php-mbstring
```
![image](https://github.com/eggsy3011/train-4/assets/108015833/627f4b24-1b35-4362-a5dc-652f7af0e71b)


# 4. Cài đặt MariaDB #
```
yum install -y mariadb mariadb-serverCopied!
```

![image](https://github.com/eggsy3011/train-4/assets/108015833/28fc8193-7f06-4689-a36f-0154181492c0)

Start MariaDB và bật khi khởi động:
```
systemctl start mariadb.service
```
```
systemctl enable mariadb.service
```
![image](https://github.com/eggsy3011/train-4/assets/108015833/f7c265ce-373c-457a-b384-0f291d0bf7dc)


# Cài đặt ban đầu và cấu hình mật khẩu cho user root MariaDB
```
mysql_secure_installation
```
# Thực hiện các câu hỏi tiếp theo khi chạy lệnh này:`

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


# 5. Cấu hình NGINX #
 
Mở file config của NGINX để chỉnh sửa cấu hình:
```
nano /etc/nginx/nginx.conf
```
Chỉnh worker_processes bằng với số CPU trên server của bạn

![image](https://github.com/eggsy3011/train-4/assets/108015833/bf9fd1a8-44ca-4f48-b9b6-2a843e285f7d)


Cấu hình nginx virtual hosts
```
nano /etc/nginx/conf.d/default.conf
```
Sửa lại nội dung file như sau:
```
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
```

# pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
```
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
```

Lưu lại và restart dịch vụ để áp dụng cấu hình.

![image](https://github.com/eggsy3011/train-4/assets/108015833/8754ee0b-befe-4a9a-8d57-39c69ba08930)

```
systemctl stop nginx.service
```
```
systemctl start nginx.service
```
![image](https://github.com/eggsy3011/train-4/assets/108015833/24e1aed1-6c41-426d-bd2d-952474ff924e)


# 6. Cấu hình PHP-FPM #

Chỉnh sửa user và group
```
nano /etc/php-fpm.d/www.conf
```
```
user = nginx
group = nginx
```
![image](https://github.com/eggsy3011/train-4/assets/108015833/0e676491-9738-4424-b42d-a54c903f716f)

# Restart dịch vụ php-fpm
```
systemctl stop php-fpm.service
```
```
systemctl start php-fpm.service
```
![image](https://github.com/eggsy3011/train-4/assets/108015833/9bc165ec-ca10-4c4f-a168-c034a5b86186)

# 7. Cấu hình firewalld #
```
firewall-cmd --permanent --add-port=80/tcp
```
```
firewall-cmd --permanent --add-port=443/tcp
```
```
firewall-cmd –reload
```
# 8. Kiểm tra hoạt động #

Chạy một file info.php để kiểm tra có Web Server có hoạt động hay không
```
nano /var/www/html/info.php
```
Thêm đoạn sau và lưu lại:
```
<?php
phpinfo();
?>
```
![image](https://github.com/eggsy3011/train-4/assets/108015833/6d2cc326-9cc7-47dc-934f-4468f43a4ef6)

![image](https://github.com/eggsy3011/train-4/assets/108015833/005a010c-e3ed-4013-8a28-fe935e3ded53)

# Wordpress:
```
cd /tmp
```
```
wget https://wordpress.org/latest.tar.gz
```
![image](https://github.com/eggsy3011/train-4/assets/108015833/db56b337-69b0-4b5c-aef4-22cbc3b4c7f1)
```
tar -xzf latest.tar.gz
```
```
sudo mv wordpress/* /var/www/your_domain
```
![image](https://github.com/eggsy3011/train-4/assets/108015833/93ff5e69-d169-4628-8542-6b550d6c7638)

![image](https://github.com/eggsy3011/train-4/assets/108015833/ca125776-9d1c-4179-9100-5847b4aa79d1)

# Cấp quyền thư mục:
```
sudo chown -R nginx:nginx /var/www/your_domain
```
```
sudo chmod -R 755 /var/www/your_domain
```
![image](https://github.com/eggsy3011/train-4/assets/108015833/c63b728c-308c-4d67-abb8-7a17bf6c98d7)
# Cấu hình WordPress:
# Tạo file cấu hình WordPress:
```
sudo cp /var/www/your_domain/wp-config-sample.php /var/www/your_domain/wp-config.php
```
```
sudo vi /var/www/your_domain/wp-config.php
```
![image](https://github.com/eggsy3011/train-4/assets/108015833/4304a67f-e0c0-4ad6-a174-4bb61669fd52)

![image](https://github.com/eggsy3011/train-4/assets/108015833/3364e376-c2c7-44e3-b64a-77345633d0fc)


# Bài tập 2: 

# Sử dụng 1 vps cài đặt mô hình LEMP 

# Cài đặt Apache:
```
sudo yum install httpd -y
```
![image](https://github.com/eggsy3011/train-4/assets/108015833/a6cc37c0-6482-4697-b177-87439b5008b5)

# Khởi động và bật Apache khởi động cùng hệ thống:
```
sudo systemctl start httpd
```
```
sudo systemctl enable httpd
```
![image](https://github.com/eggsy3011/train-4/assets/108015833/8cbfe435-d9de-401c-8c11-fa612c398caf)


# Cài đặt MySQL (MariaDB):

![image](https://github.com/eggsy3011/train-4/assets/108015833/5e3c7389-c234-4c62-81dc-3346e11a1fc1)

# Thiết lập bảo mật cho MariaDB:
```
sudo mysql_secure_installatio
```
![image](https://github.com/eggsy3011/train-4/assets/108015833/16ea6dd4-90c3-4a5d-a093-8f96a8981bd1)

# Cài đặt PHP:
```
sudo yum install php php-mysql -y
```
# Khởi động lại Apache để PHP hoạt động:
```
sudo systemctl restart httpd
```
# Kiểm tra PHP:

# Tạo một tệp thông tin PHP để kiểm tra PHP hoạt động đúng cách:
```
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
```
![image](https://github.com/eggsy3011/train-4/assets/108015833/43a16fda-3fa2-4dea-93c1-6036e55944b2)

 # Tạo Virtual Hosts cho WordPress và Laravel
   
# Tạo thư mục cho các trang web:
```
sudo mkdir -p /var/www/<name>.wordpress.zonecloud.co
```
```
sudo mkdir -p /var/www/<name>.laravel.zonecloud.co
```
![image](https://github.com/eggsy3011/train-4/assets/108015833/8c85cde7-1b63-4abd-b491-cd5fce7061aa)

# Thiết lập quyền sở hữu và phân quyền:
```
sudo chown -R $USER:$USER /var/www/example.wordpress.zonecloud.co
```
```
sudo chown -R $USER:$USER /var/www/example.laravel.zonecloud.co
```
```
sudo chmod -R 755 /var/www
```
![image](https://github.com/eggsy3011/train-4/assets/108015833/9a29cf46-e776-4a23-a462-0c349bec3a06)

# Tạo tệp cấu hình Virtual Host cho WordPress:
```
sudo nano /etc/httpd/conf.d/example.wordpress.zonecloud.co.conf
```
# Trong nano ta config: 
```
<VirtualHost *:80>
    ServerAdmin admin@zonecloud.co
    DocumentRoot /var/www/example.wordpress.zonecloud.co
    ServerName example.wordpress.zonecloud.co
    ErrorLog /var/log/httpd/example.wordpress.zonecloud.co-error.log
    CustomLog /var/log/httpd/example.wordpress.zonecloud.co-access.log combined
</VirtualHost>
```
# Tạo tệp cấu hình Virtual Host cho Laravel:
```
sudo nano /etc/httpd/conf.d/example.laravel.zonecloud.co.conf
```
Trong nano ta config: 
```
<VirtualHost *:80>
    ServerAdmin admin@zonecloud.co
    DocumentRoot /var/www/example.laravel.zonecloud.co/public
    ServerName example.laravel.zonecloud.co
    ErrorLog /var/log/httpd/example.laravel.zonecloud.co-error.log
    CustomLog /var/log/httpd/example.laravel.zonecloud.co-access.log combined
</VirtualHost>
```

# Cài đặt Certbot:
```
sudo yum install certbot python3-certbot-apache -y
```
![image](https://github.com/eggsy3011/train-4/assets/108015833/8c62dc4e-e181-4eb7-9710-4c9ce143e74c)
```
sudo certbot --apache -d example.wordpress.zonecloud.co
```
```
sudo certbot --apache -d example.laravel.zonecloud.co
```

Source: wordpress -> <name>.wordpress.zonecloud.co laravel -> <name>.laravel.zonecloud.co

Setup SSL cho cả 2 domain




