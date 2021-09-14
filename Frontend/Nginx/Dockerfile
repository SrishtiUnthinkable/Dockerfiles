 FROM ubuntu:18.04
 ENV DEBIAN_FRONTEND noninteractive
 RUN apt update &&  apt -y install software-properties-common && add-apt-repository ppa:ondrej/php
 RUN apt-get update && apt -y install php7.4 && apt -y install php7.4-fpm && apt install nginx -y
 COPY ./config/testing.snubes.com.conf /etc/nginx/conf.d/default.conf
 WORKDIR /var/www/frontend
 COPY . .
 RUN mkdir -p /etc/ssl/certs/snubes/
 COPY ./config/ssl/* /etc/nginx/ssl/snubes_com/
 CMD /etc/init.d/php7.4-fpm restart && nginx -g "daemon off;"
 EXPOSE 80 443 9000
