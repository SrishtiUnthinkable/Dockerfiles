FROM node:9.11.2 as admin

RUN apt update

RUN mkdir /root/.ssh/
COPY key.pem /root/.ssh/id_rsa
RUN ls /root/.ssh/
RUN chmod 400 /root/.ssh/id_rsa
RUN touch /root/.ssh/known_hosts
RUN ssh-keyscan github.com >> /root/.ssh/known_hosts

RUN mkdir /var/www
RUN mkdir /var/www/admin

WORKDIR /var/www/admin

COPY ["package.json", "package-lock.json", "./"]

RUN npm install

COPY . .

CMD ["npm", "run", "dev"]

FROM test:adminn
RUN apt update && apt upgrade -y && apt install net-tools -y && apt install vim -y
RUN apt -y install lsb-release apt-transport-https ca-certificates && apt install -y wget 
RUN wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg
RUN echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/php.list
RUN apt update && apt -y install php7.4 && apt -y install php7.4-fpm
RUN mkdir -p /var/www/admin
RUN mkdir -p /var/www/cdn
WORKDIR /var/www/admin
COPY . .
COPY ./config/admin.dev.snubes.com.conf /etc/nginx/conf.d/default.conf
#COPY ./config/cdn.dev.snubes.com.conf /etc/nginx/conf.d/cdn.dev.snubes.com.conf
RUN mkdir -p /etc/ssl/certs/snubes/
COPY ./config/ssl/* /etc/ssl/certs/snubes/
COPY --from=admin /var/www/admin/public /var/www/cdn
CMD /etc/init.d/php7.4-fpm restart && nginx -g "daemon off;"
EXPOSE 80 443 9000
