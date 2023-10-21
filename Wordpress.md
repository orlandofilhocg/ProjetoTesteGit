Comandos do OPENSSL:

====================
sudo openssl req -x509 -sha256 -days 365 -nodes -newkey rsa:2048 -keyout rootCA.key -out rootCA.crt
 
sudo openssl genrsa -out certificado.key 2048
 
sudo openssl req -new -key certificado.key -out certificado.csr
 
sudo openssl x509 -req -in certificado.csr -CA rootCA.crt -CAkey rootCA.key -CAcreateserial -out certificado.crt -days 365 -sha256






WORDPRESS - Install

sudo apt update && sudo apt upgrade -y

sudo apt install nginx -y

sudo apt install php-fpm php-mysql -y

/etc/nginx/sites-enabled/default
============================================================
server {
listen 80;
server_name uepb.com;
root /var/www/uepb.com;
index index.php index.html;


location / {
try_files $uri $uri/ /index.php?$args;
}

location ~ \.php$ {
include snippets/fastcgi-php.conf;
fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
}
}
===============================================================
sudo ln -s /etc/nginx/sites-available/uepb.com.conf /etc/nginx/sites-enabled/


Instalar MysqlServer

sudo apt install mysql-server -y

sudo mysql

Alterar a senha root do MySQL
====================================

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password';

mysql -u root -p

CREATE DATABASE wordpress;
CREATE USER 'wordpressuser'@'localhost' IDENTIFIED BY 'password';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wordpressuser'@'localhost';
FLUSH PRIVILEGES;

wget https://wordpress.org/latest.tar.gz

tar -xzvf latest.tar.gz

sudo mv wordpress /var/www/uepb.com

sudo cp /var/www/uepb.com/wp-config-sample.php /var/www/uepb.com/wp-config.php

sudo nano /var/www/uepb.com/wp-config.php


define('DB_NAME', 'wordpress');
define('DB_USER', 'wordpressuser');
define('DB_PASSWORD', 'password');
define('DB_HOST', 'localhost');
define('DB_CHARSET', 'utf8mb4');
define('DB_COLLATE', '');

sudo chown -R www-data:www-data /var/www/uepb.com
sudo find /var/www/uepb.com/ -type d -exec chmod 755 {} \;
sudo find /var/www/uepb.com/ -type f -exec chmod 644 {} \;
