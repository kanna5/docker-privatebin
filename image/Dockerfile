FROM php:8.2-fpm-alpine

RUN set -xe; \
# nginx
    apk upgrade; \
    apk add --no-cache nginx tzdata; \
    ln -sf /dev/stdout /var/log/nginx/access.log; \
    ln -sf /dev/stderr /var/log/nginx/error.log; \
    \
# gd extension
    apk add --virtual .php-ext-deps zlib libpng; \
    apk add --virtual .php-ext-build-deps zlib-dev libpng-dev; \
    docker-php-ext-configure gd; \
    docker-php-ext-install -j$(nproc) gd; \
    docker-php-ext-enable opcache; \
    apk del .php-ext-build-deps; \
    rm /var/cache/apk/*; \
    mv -v "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

COPY ./php.ini "$PHP_INI_DIR/conf.d/"
COPY ./nginx_http.conf /etc/nginx/http.d/default.conf

RUN set -xe; \
# Copy application files
    mkdir -p /usr/local/privatebin; \
    cd /usr/local/privatebin; \
    curl -sSLf 'https://github.com/PrivateBin/PrivateBin/archive/refs/tags/1.7.8.tar.gz' \
        | tar --strip-components=1 --no-same-owner -xz; \
    install -d -o www-data -g www-data data;

WORKDIR /usr/local/privatebin

EXPOSE 80/tcp
