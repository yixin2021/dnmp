ARG PHP_VERSION
FROM php:${PHP_VERSION}-fpm-alpine

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

VOLUME /var/www
WORKDIR /var/www

EXPOSE 9000

