ARG PHP_VERSION
FROM php:${PHP_VERSION}-fpm-alpine

# 配置阿里云镜像
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories
# 设置时区
#RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo 'Asia/Shanghai'>/etc/timezone

# 添加依赖
RUN apk --update add \
         git \
         autoconf \
         gcc  \
         g++  \
         make  \
         openssl  \
         openssl-dev \
         libmcrypt-dev  \
         freetype-dev  \
         libjpeg-turbo-dev \
         freetype-dev \
         libjpeg-turbo-dev \
         libpng-dev \
 && docker-php-ext-install mysqli pdo pdo_mysql mbstring bcmath zip opcache sockets \
 && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/ \
 && docker-php-ext-install -j$(nproc) gd


# 安装redis扩展
ARG INSTALL_REDIS=false
ARG REDIS_VERSION
RUN if [ ${INSTALL_REDIS} = true ]; then \
    curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/${REDIS_VERSION}.tar.gz \
    && tar xfz /tmp/redis.tar.gz \
    && rm -r /tmp/redis.tar.gz \
    && mkdir -p /usr/src/php/ext \
    && mv phpredis-${REDIS_VERSION} /usr/src/php/ext/redis \
    && docker-php-ext-install redis \
;fi


# 安装 swoole扩展
ARG INSTALL_SWOOLE=false
ARG SWOOLE_VERSION
RUN if [ ${INSTALL_SWOOLE} = true ]; then \
    cd /root && \
    curl -o /tmp/swoole.tar.gz https://github.com/swoole/swoole-src/archive/v${SWOOLE_VERSION}.tar.gz -L && \
    tar zxvf /tmp/swoole.tar.gz && cd swoole-src* && \
    phpize && \
    ./configure \
    --enable-coroutine \
    --enable-openssl  \
    --enable-http2  \
    --enable-async-redis \
    --enable-sockets \
    --enable-mysqlnd && \
    make && make install && \
    docker-php-ext-enable swoole && \
    echo "swoole.fast_serialize=On" >> /usr/local/etc/php/conf.d/docker-php-ext-swoole-serialize.ini && \
    rm -rf /tmp/* \
;fi


# 安装 pnctl
ARG INSTALL_PCNTL=false
RUN if [ ${INSTALL_PCNTL} = true ]; then \
    docker-php-ext-install pcntl \
;fi

# 安装xdebug
ARG INSTALL_XDEBUG=false
ARG XDEBUG_VERSION
RUN if [ ${INSTALL_XDEBUG} = true ]; then \
    pecl install xdebug-${XDEBUG_VERSION} \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable xdebug \
;fi

# 安装composer
RUN curl -o /usr/bin/composer https://mirrors.aliyun.com/composer/composer.phar \
    && chmod +x /usr/bin/composer \
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer

VOLUME /var/www
WORKDIR /var/www

EXPOSE 9000

CMD ["php-fpm"]

