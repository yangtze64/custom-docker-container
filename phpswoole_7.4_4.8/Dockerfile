FROM php:7.4.29-zts
# 安装依赖
RUN apt-get update --fix-missing && apt-get install -y curl libzip-dev bison autoconf build-essential pkg-config git-core\
       libltdl-dev libbz2-dev libxml2-dev libxslt1-dev libssl-dev libicu-dev libpspell-dev\
       libmcrypt-dev libpng-dev libjpeg-dev libfreetype6-dev\
       libreadline-dev openssl libcurl4-openssl-dev librecode-dev libsqlite3-dev\
       libonig-dev zlib1g-dev libc-ares-dev

RUN docker-php-source extract

RUN curl -L -o /tmp/mcrypt.tgz https://pecl.php.net/get/mcrypt-1.0.3.tgz && tar zxvf /tmp/mcrypt.tgz && mv mcrypt-1.0.3 /usr/src/php/ext/mcrypt && rm -rf /tmp/mcrypt.tgz

RUN curl -L -o /tmp/igbinary.tar.gz https://mirror.ghproxy.com/https://github.com/igbinary/igbinary/archive/refs/tags/3.2.7.tar.gz && tar -zxvf /tmp/igbinary.tar.gz && mv igbinary-3.2.7 /usr/src/php/ext/igbinary && rm -rf /tmp/igbinary.tar.gz

# 安装扩展
RUN docker-php-ext-install -j$(nproc) mysqli pdo_mysql bz2 zip bcmath sockets pcntl mcrypt igbinary
RUN docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ && docker-php-ext-install gd
# 安装redis扩展
RUN curl -L -o /tmp/phpredis.tar.gz https://mirror.ghproxy.com/https://github.com/phpredis/phpredis/archive/refs/tags/5.3.7.tar.gz && tar -zxvf /tmp/phpredis.tar.gz && mv phpredis-5.3.7 /usr/src/php/ext/phpredis && rm -rf /tmp/phpredis.tar.gz

RUN docker-php-ext-configure phpredis --enable-redis-igbinary && docker-php-ext-install phpredis

# 安装swoole扩展
RUN curl -L -o /tmp/swoole.tar.gz https://mirror.ghproxy.com/https://github.com/swoole/swoole-src/archive/refs/tags/v4.8.10.tar.gz && tar -zxvf /tmp/swoole.tar.gz && mv swoole-src-4.8.10 /usr/src/php/ext/swoole && rm -rf /tmp/swoole.tar.gz

RUN docker-php-ext-configure swoole --enable-openssl --with-openssl-dir=/usr/lib/ssl --enable-http2 --enable-swoole-json --enable-swoole-curl --enable-cares && docker-php-ext-install swoole

# RUN cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini

# 安装composer
RUN curl -L -o /usr/local/bin/composer https://getcomposer.org/download/1.10.24/composer.phar && chmod +x /usr/local/bin/composer
RUN composer config -g repo.packagist composer https://mirrors.cloud.tencent.com/composer
# 安装kafka扩展
RUN curl -L -o /tmp/librdkafka.tar.gz https://mirror.ghproxy.com/https://github.com/edenhill/librdkafka/archive/refs/tags/v1.6.2.tar.gz
RUN cd /tmp && tar -zxvf librdkafka.tar.gz && cd librdkafka-1.6.2 && ./configure && make && make install && cd ~ && rm -rf /tmp/librdkafka*

RUN curl -L -o /tmp/rdkafka.tar.gz https://mirror.ghproxy.com/https://github.com/arnaud-lb/php-rdkafka/archive/refs/tags/6.0.1.tar.gz && tar -zxvf /tmp/rdkafka.tar.gz && mv php-rdkafka-6.0.1 /usr/src/php/ext/rdkafka && rm -rf /tmp/rdkafka.tar.gz

RUN docker-php-ext-install rdkafka

RUN docker-php-source delete

RUN php -v && composer -V



