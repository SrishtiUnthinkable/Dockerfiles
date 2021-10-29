FROM php:7.4-fpm
RUN apt-get update \
  && apt-get install -y --no-install-recommends libpq-dev \
  && docker-php-ext-install mysqli pdo_pgsql pdo_mysql \
  && apt-get install -y zip unzip git 
#Install Redis-Server
RUN apt-get update \
&& apt-get install redis-server -y \
&& service redis-server stop \
&& service redis-server start
# Install PHP Redis extension
RUN pecl install -o -f redis \  
  &&  rm -rf /tmp/pear \
  &&  docker-php-ext-enable redis

#Install wkhtmlpdf  
RUN apt-get install -y \
libxrender1 \
libfontconfig1 \
libx11-dev \
libjpeg62 \
libxtst6 \
wget \
&& wget https://github.com/h4cc/wkhtmltopdf-amd64/blob/master/bin/wkhtmltopdf-amd64?raw=true -O /usr/bin/wkhtmltopdf \
&& chmod +x /usr/bin/wkhtmltopdf
# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
COPY ./entry_point.sh /
WORKDIR /var/www/backend
COPY  . .
RUN chmod +x /entry_point.sh
ARG ENV
ARG HOST_USER
ARG HOST_USER_GROUP
USER ${HOST_USER}:${HOST_USER_GROUP}
ENTRYPOINT ["/entry_point.sh"]
