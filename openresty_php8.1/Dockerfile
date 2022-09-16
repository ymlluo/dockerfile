FROM alpine:3.16

ENV OPENRESTY_DEPS="g++ gcc make perl-dev pcre-dev openssl-dev zlib-dev "
ENV OPENRESTY_VERSION=1.21.4.1
ENV TZ=Asia/Shanghai
RUN apk add --no-cache tzdata ca-certificates xz openssl curl gzip tar $OPENRESTY_DEPS\
    && rm -f /etc/localtime \
    && ln -s /usr/share/zoneinfo/${TZ}} /etc/localtime \
    && cd /tmp \
    && curl -sSLf  -o openresty-$OPENRESTY_VERSION.tar.gz https://openresty.org/download/openresty-$OPENRESTY_VERSION.tar.gz \
    && tar -zxvf openresty-$OPENRESTY_VERSION.tar.gz\
    && cd openresty-$OPENRESTY_VERSION/ \
    && ./configure -j2 \
    && make -j2 \
    &&  make install \
    && export PATH=/usr/local/openresty/bin:$PATH\
    && mkdir -p /usr/local/openresty/nginx/sites/ \
    && mkdir -p /var/log/openresty \
    &&  apk del $OPENRESTY_DEPS \
    && rm  -rf /tmp/* \
    && rm -rf /var/cache/apk/*
ENV PHPIZE_DEPS="autoconf dpkg-dev dpkg file g++ gcc libc-dev make pkgconf re2c pcre pcre-dev zlib zlib-dev openssl-dev php81-dev "
RUN apk add --no-cache $PHPIZE_DEPS ncdu   php81 php81-cli php81-fpm php81-soap php81-openssl php81-gmp php81-pdo_odbc php81-json php81-dom php81-pdo php81-zip php81-mysqli php81-sqlite3 php81-apcu php81-pdo_pgsql php81-bcmath php81-gd  php81-odbc php81-pdo_mysql php81-pdo_sqlite php81-gettext php81-xmlreader  php81-bz2  php81-iconv php81-pdo_dblib php81-curl php81-ctype php81-opcache php81-pear php81-session composer \
    && ln -s /usr/bin/php81 /usr/local/bin/php \
    && ln -s /usr/bin/pecl81 /usr/bin/pecl \
    && ln -s /usr/sbin/php-fpm81 /usr/sbin/php-fpm \
    && ln -s /usr/bin/phpize81 /usr/local/bin/phpize \
    && ln -s /usr/bin/php-config81 /usr/local/bin/php-config \
    && pecl install -o -f redis \
    && echo "extension=redis.so" > /etc/php81/conf.d/redis.ini \
    && pecl install -o -f sqlsrv \
    && pecl install pdo_sqlsrv \
    && echo extension=pdo_sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/10_pdo_sqlsrv.ini \
    && echo extension=sqlsrv.so >> `php --ini | grep "Scan for additional .ini files" | sed -e "s|.*:\s*||"`/20_sqlsrv.ini \
    && curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/msodbcsql17_17.10.1.1-1_amd64.apk \
    && curl -O https://download.microsoft.com/download/e/4/e/e4e67866-dffd-428c-aac7-8d28ddafb39b/mssql-tools_17.10.1.1-1_amd64.apk \
    && apk add --allow-untrusted msodbcsql17_17.10.1.1-1_amd64.apk \
    && apk add --allow-untrusted mssql-tools_17.10.1.1-1_amd64.apk \
    && addgroup -S www \
    && adduser -S www -G www \
    &&  apk del $PHPIZE_DEPS \
    && rm  -rf /tmp/* \
    && rm -rf /var/cache/apk/*
RUN apk add --no-cache nodejs \
    && rm -rf /var/cache/apk/*

# RUN set -eux; 	adduser -u 82 -D -S -G www www \
#     && mkdir -p "$PHP_INI_DIR/conf.d"; 	[ ! -d /var/www/html ]; 	mkdir -p /var/www/html; 	chown www-data:www-data /var/www/html; 	chmod 777 /var/www/html

CMD [ "/bin/sh" ]