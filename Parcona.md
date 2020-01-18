Setting up Mariadb with docker

https://severalnines.com/blog/how-deploy-mariadb-server-docker-container

mkdir data config

docker run -d --name mariadb1 \
-p 33061:3306 \
-v ~/~/Documents/magentodb/config:/etc/mysql/conf.d \
-v ~/~/Documents/magentodb/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root123 \
-e MYSQL_DATABASE=dbtest \
mariadb

docker exec -it mariadb1 bash

mysql -p -e "SHOW DATABASES;"

sudo mysql -u root -p

Create database magdb;
CREATE USER magousr@'localhost' IDENTIFIED BY '123abc';
grant all privileges on magdb.* to 'magousr'@localhost ;
FLUSH PRIVILEGES;

Accessing using valentina


Setting up webserver 

sudo apt-cache policy nginx
sudo apt-get -y install nginx
sudo apt-cache policy php7.2
sudo apt-get install php7.2-fpm php7.2-cli php7.2 php7.2-common php7.2-gd php7.2-mysql php7.2-curl php7.2-intl php7.2-xsl php7.2-mbstring php7.2-zip php7.2-bcmath php7.2-iconv php7.2-soap

sudo php -v
sudo php -me

vim /etc/php/7.2/fpm/php.ini
memory_limit = 2G
max_execution_time = 1800
zlib.output_compression = O

vim /etc/php/7.2/cli/php.ini
memory_limit = 2G
max_execution_time = 1800
zlib.output_compression = O

sudo systemctl restart php7.2-fpm

installing composer
sudo curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/bin --filename=composer

sudo adduser deploy
sudo mkdir -p /var/www/html/webapp

chown -R deploy:www-data /var/www/html/webapp

sudo su deploy
cd /var/www/html/webapp

composer create-project --repository=https://repo.magento.com/ magento/project-community-edition=2.3.3 .



cp /var/www/html/webapp/nginx.conf.sample /etc/nginx/magento.conf
sudo vim /etc/nginx/sites-available/magento
  upstream fastcgi_backend {
     server  unix:/run/php/php7.2-fpm.sock;
 }
server {
listen 80;
     server_name magentotest.fosslinux.com;
     set $MAGE_ROOT /var/www/html/webapp;
     include /etc/nginx/magento.conf;
 }


sudo ln -s /etc/nginx/sites-available/magento /etc/nginx/sites-enabled
sudo nginx -t
sudo systemctl restart nginx

sudo cd /var/www/html/webapp
sudo chmod -R 775 var/ generated/ pub/ app/ vendor/