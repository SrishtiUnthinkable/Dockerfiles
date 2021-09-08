FROM node:9.11.2 as front

RUN apt update && apt install rsync -y

RUN mkdir -p /var/www/frontend

WORKDIR /var/www/frontend

COPY ["package.json", "package-lock.json", "./"]

RUN npm install

COPY . .

CMD ["npm", "run", "dev"]

FROM test:front
RUN apt update && apt upgrade -y && apt install net-tools -y && apt install vim -y
RUN apt -y install lsb-release apt-transport-https ca-certificates && apt install -y wget 
RUN wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
RUN echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/php.list
RUN apt update && apt -y install php7.4 && apt -y install php7.4-fpm 

RUN mkdir -p /var/www/cdn
WORKDIR /var/www/frontend
COPY --from=front /var/www/frontend/ /var/www/frontend
COPY ./config/testing.snubes.com.conf /etc/nginx/conf.d/default.conf
# COPY --from=front /var/www/frontend/public /var/www/frontend/public
# COPY ./config/cdn.dev.snubes.com.conf /etc/nginx/conf.d/cdn.dev.snubes.com.conf
RUN mkdir -p /etc/ssl/certs/snubes/
COPY ./config/ssl/snubes_com/* /etc/nginx/ssl/snubes_com/
COPY --from=front /var/www/frontend/public /var/www/cdn
CMD /etc/init.d/php7.4-fpm restart && nginx -g "daemon off;"
EXPOSE 80 443 9000
