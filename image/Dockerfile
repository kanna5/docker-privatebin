FROM php:8.2-fpm-alpine

ENV PRIVATE_BIN_VERSION=2.0.0

RUN <<-EOF
    set -xe

    # nginx
    apk upgrade
    apk add --no-cache nginx tzdata
    ln -sf /dev/stdout /var/log/nginx/access.log
    ln -sf /dev/stderr /var/log/nginx/error.log

    rm /var/cache/apk/*
    mv -v "${PHP_INI_DIR}/php.ini-production" "${PHP_INI_DIR}/php.ini"
EOF

COPY ./php.ini "${PHP_INI_DIR}/conf.d/"
COPY ./nginx_http.conf /etc/nginx/http.d/default.conf

RUN <<-EOF
    set -xe
    # Copy application files
    mkdir -p /usr/local/privatebin
    cd /usr/local/privatebin
    curl -sSLf "https://github.com/PrivateBin/PrivateBin/archive/refs/tags/${PRIVATE_BIN_VERSION}.tar.gz" \
        | tar --strip-components=1 --no-same-owner -xz
    install -d -o www-data -g www-data data;
EOF

WORKDIR /usr/local/privatebin

EXPOSE 80/tcp
