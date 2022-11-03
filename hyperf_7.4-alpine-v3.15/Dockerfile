FROM php:7.4.26-zts-alpine3.15
LABEL maintainer="594998624@qq.com"
RUN set -ex && sed -i 's/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apk/repositories && apk update  \
    && apk add --no-cache ca-certificates curl wget tar xz pcre libstdc++ openssl git bash \
    libmcrypt libpng freetype libjpeg-turbo libzip librdkafka tzdata \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && apk del tzdata \
    && apk add --no-cache --virtual .build-deps file libpng-dev freetype-dev libjpeg-turbo-dev libwebp-dev libxpm-dev  \
    zlib-dev libzip-dev autoconf gcc g++ make libc-dev dpkg-dev re2c automake libmcrypt-dev openssl-dev pkgconf  \
    pcre2 pcre2-dev librdkafka-dev \
    && docker-php-source extract && cd /tmp/ \
    && curl -SL -o mcrypt.tgz https://pecl.php.net/get/mcrypt-1.0.3.tgz && tar zxvf mcrypt.tgz && mv mcrypt-1.0.3 /usr/src/php/ext/mcrypt \
    && curl -SL -o igbinary.tar.gz https://ghproxy.com/https://github.com/igbinary/igbinary/archive/refs/tags/3.2.7.tar.gz && tar -zxvf igbinary.tar.gz && mv igbinary-3.2.7 /usr/src/php/ext/igbinary \
    && curl -SL -o rdkafka.tar.gz https://ghproxy.com/https://github.com/arnaud-lb/php-rdkafka/archive/refs/tags/6.0.1.tar.gz && tar -zxvf rdkafka.tar.gz && mv php-rdkafka-6.0.1 /usr/src/php/ext/rdkafka \
    && docker-php-ext-install -j$(nproc) mysqli pdo_mysql bz2 zip bcmath sockets pcntl mcrypt igbinary rdkafka \
    && docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ && docker-php-ext-install gd \
    && curl -SL -o phpredis.tar.gz https://ghproxy.com/https://github.com/phpredis/phpredis/archive/refs/tags/5.3.7.tar.gz && tar -zxvf phpredis.tar.gz && mv phpredis-5.3.7 /usr/src/php/ext/phpredis \
    && docker-php-ext-configure phpredis --enable-redis-igbinary && docker-php-ext-install phpredis \
    && curl -SL -o swoole.tar.gz https://ghproxy.com/https://github.com/swoole/swoole-src/archive/refs/tags/v4.8.10.tar.gz && tar -zxvf swoole.tar.gz && mv swoole-src-4.8.10 /usr/src/php/ext/swoole \
    && docker-php-ext-configure swoole --enable-openssl --with-openssl-dir=/usr/lib/ssl --enable-http2 --enable-swoole-json && docker-php-ext-install swoole \
    && cd / && docker-php-source delete \
    && apk del .build-deps && rm -rf /var/cache/apk/* /tmp/* \
    && echo -e "\033[42;37m Build Completed :).\033[0m\n"

