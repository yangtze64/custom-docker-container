FROM hyperf/hyperf:7.4-alpine-v3.15-swoole-v4.8.8
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apk/repositories  \
    && apk add --no-cache --virtual .build-deps  \
    curl file libpng-dev freetype-dev libjpeg-turbo-dev libwebp-dev libxpm-dev zlib-dev libzip-dev autoconf  \
    gcc g++ make libc-dev dpkg-dev re2c automake libmcrypt-dev openssl-dev pcre2 pcre2-dev pkgconf  \
    tzdata php7-dev php7-pear \
    && cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime  \
    && apk add --no-cache librdkafka-dev  \
    && cd /tmp && curl -SL -o rdkafka.tar.gz https://ghproxy.com/https://github.com/arnaud-lb/php-rdkafka/archive/refs/tags/6.0.1.tar.gz  \
    && tar -zxvf rdkafka.tar.gz && cd php-rdkafka-6.0.1  \
    && ln -s /usr/bin/phpize7 /usr/local/bin/phpize && ln -s /usr/bin/php-config7 /usr/local/bin/php-config  \
    && (phpize && ./configure && make -s -j$(nproc) && make install) && echo "extension=rdkafka.so" > /etc/php7/conf.d/51_rdkafka.ini \
    && apk del .build-deps && rm -rf /var/cache/apk/* /tmp/* /usr/share/man /usr/local/bin/php* \
    && echo -e "\033[42;37m Build Completed :).\033[0m\n"