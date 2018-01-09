FROM php:5.6-fpm

MAINTAINER Kirill Rakauchuk "trw2NgYfmDxnZGu@gmail.com"

RUN apt-get update
RUN apt-get install -y --no-install-recommends

RUN apt-get install -y libfreetype6-dev libjpeg62-turbo-dev libpng12-dev
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install -j$(nproc) gd

RUN apt-get install -y libicu-dev
RUN docker-php-ext-install -j$(nproc) intl

RUN apt-get install -y mysql-client
RUN apt-get install -y libmcrypt-dev
RUN docker-php-ext-install mcrypt
RUN docker-php-ext-install zip
RUN docker-php-ext-install pcntl
RUN docker-php-ext-install opcache
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install mysqli

RUN apt-get install -y libcurl3
RUN apt-get install -y libcurl4-openssl-dev
RUN docker-php-ext-install curl
RUN docker-php-ext-install sockets

RUN apt-get install -y libpq-dev
RUN docker-php-ext-configure pgsql \
    -with-pgsql=/usr/local/pgsql
RUN docker-php-ext-install pgsql
RUN docker-php-ext-install pdo_pgsql

RUN apt-get install -y libxml2-dev
RUN apt-get install -y libxslt-dev
RUN docker-php-ext-install xsl
RUN docker-php-ext-install xmlrpc
RUN docker-php-ext-install soap

RUN apt-get install -y --no-install-recommends apt-utils libz-dev libmemcached-dev && \
    pecl install memcached-2.2.0
RUN echo extension=memcached.so >> $PHP_INI_DIR/conf.d/memcached.ini

COPY config/fpm/php-fpm.conf $PHP_INI_DIR/../php-fpm.conf
COPY config/fpm/www.conf $PHP_INI_DIR/../php-fpm.d/www.conf

RUN rm -v $PHP_INI_DIR/../php-fpm.d/zz-docker.conf
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* /usr/share/doc/*

RUN usermod -u 1000 www-data
RUN usermod -a -G users www-data

WORKDIR /var/www/html

CMD ["php-fpm"]

EXPOSE 9000
