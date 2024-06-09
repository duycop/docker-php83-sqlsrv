# Docker-php83-sqlsrv
docker desktop, run on for window os.

# Condition
Prerequisites : successfully installed docker desktop on windows os : https://www.docker.com/products/docker-desktop/

# Pull
docker pull tudonghoa/php_sqlsrv

# Install
docker run -d --name myweb --privileged --restart=always -v E:\Docker\myweb:/app -p 80:80 tudonghoa/php_sqlsrv


# build

#0. tạo network `duycop_network`
docker network create --subnet=192.168.80.0/24 --gateway=192.168.80.1 --ip-range=192.168.80.0/24 --driver=bridge duycop_network

#1. tải image docker
docker pull webdevops/php-nginx:8.3-alpine

#2. tạo thư mục chứa web
mkdir "E:\Docker\nginx\web2"

#3.chạy docker vào thư mục 
docker run -d --name php-nginx-web2 --privileged --restart=always -v E:\Docker\nginx\web2:/app --network=duycop_network --ip 192.168.80.27 -p 8088:80 webdevops/php-nginx:8.3-alpine

#4. chui vào trong docker:
docker exec -u root -t -i php-nginx-web2 /bin/bash

#5 cài đặt thư viện g++
apk update
apk add --no-cache autoconf g++ make
apk add --no-cache unixodbc-dev

#6. biên dịch thư viện:
pecl install sqlsrv-5.12.0
pecl install pdo_sqlsrv-5.12.0

#7. tạo 2 file ini để load thư viện:
echo extension=sqlsrv.so > /usr/local/etc/php/conf.d/php_sqlsrv.ini
echo extension=pdo_sqlsrv.so > /usr/local/etc/php/conf.d/php_pdo_sqlsrv.ini

#8 ra ngoài docker
exit

#9. khởi động lại docker
docker restart php-nginx-web2

#9 sau khi buil ra image
chạy nó:
docker run -d --name php-nginx-web2 --privileged --restart=always -v E:\Docker\nginx\web2:/app --network=duycop_network --ip 192.168.80.27 -p 8088:80 webdevops/php-nginx:8.3-alpine

#10. đã push lên hub, chỉ việc kéo nó về:
docker pull tudonghoa/php_sqlsrv

#11 chạy nó
docker run -d --name php-nginx-web2 --privileged --restart=always -v E:\Docker\nginx\web2:/app --network=duycop_network --ip 192.168.80.27 -p 8088:80 tudonghoa/php_sqlsrv

# code
FROM webdevops/php-nginx:8.3-alpine

RUN apk update
RUN apk add --no-cache autoconf g++ make
RUN apk add --no-cache unixodbc-dev
RUN pecl install sqlsrv
RUN pecl install sqlsrv
RUN echo "extension=sqlsrv.so" > /usr/local/etc/php/conf.d/php_sqlsrv.ini
RUN echo "extension=pdo_sqlsrv.so" > /usr/local/etc/php/conf.d/php_pdo_sqlsrv.ini
RUN apk del autoconf g++ make

WORKDIR /app



