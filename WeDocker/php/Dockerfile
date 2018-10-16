FROM php:fpm-alpine

ENV CPHALCON_VERSION="3.4.1" \
    SWOOLE_VERSION="4.1.2" \
    REDIS_VERSION="4.1.1" \
    XDEBUG_VERSION="2.6.1" \
    GEOIP_VERSION="1.1.1" \
    MEMCACHED_VERSION="3.0.4" \
    IMAGICK_VERSION="3.4.3" \
    MONGODB_VERSION="1.5.2" \
    YAR_VERSION="2.0.4" \
    MSGPACK_VERSION="2.0.2" \
    MCRYPT_VERSION="1.0.1" \
    SITE_WORK_DIR="/sites" \
    COMPOSER_MIRROR="https://packagist.laravel-china.org" \
    ENVIRON="develop"

COPY composer /usr/local/bin/

COPY lib/cphalcon-${CPHALCON_VERSION}.tar.gz \
     lib/swoole-${SWOOLE_VERSION}.tgz \
     lib/redis-${REDIS_VERSION}.tgz \
     lib/xdebug-${XDEBUG_VERSION}.tgz \
     lib/geoip-${GEOIP_VERSION}.tgz \
     lib/memcached-${MEMCACHED_VERSION}.tgz \
     lib/imagick-${IMAGICK_VERSION}.tgz \
     lib/mongodb-${MONGODB_VERSION}.tgz \
     lib/yar-${YAR_VERSION}.tgz \
     lib/msgpack-${MSGPACK_VERSION}.tgz \
     lib/mcrypt-${MCRYPT_VERSION}.tgz /tmp/

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories \
&& apk --update add --no-cache --virtual build-dependencies \
&& apk --update add --no-cache \
    ${PHPIZE_DEPS} \
    curl-dev \
    curl \
    acl \
    git \
    shadow \	
    imagemagick-dev \
    imagemagick \
    libtool \
    libxml2-dev \
    postgresql-dev \
    postgresql-libs \
    libstdc++ \
    freetype \
    libpng \
    libjpeg-turbo \
    freetype-dev \
    libpng-dev \
    libwebp-dev \
    libjpeg-turbo-dev \
    jpeg-dev \
    libmcrypt \
    libmcrypt-dev \
    gmp-dev \
    libmemcached-dev \
    gettext-dev \
    bzip2-dev \
    geoip-dev \
    ca-certificates \
&& docker-php-ext-configure gd \
    --with-gd \
    --with-freetype-dir=/usr/include/ \
    --with-png-dir=/usr/include/ \
    --with-webp-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/ && \
    NPROC=$(getconf _NPROCESSORS_ONLN) && \
    docker-php-ext-install -j${NPROC} gd bcmath pdo_mysql pdo_pgsql exif sockets gmp soap pcntl zip mysqli calendar \
&& cd /tmp && tar xvzf cphalcon-${CPHALCON_VERSION}.tar.gz \
    && cd cphalcon-${CPHALCON_VERSION}/build \
    && sh install \
    && echo "extension=phalcon.so" > /usr/local/etc/php/conf.d/phalcon.ini \
&& cd /tmp && pecl install mcrypt-${MCRYPT_VERSION}.tgz && docker-php-ext-enable mcrypt \
&& cd /tmp && pecl install memcached-${MEMCACHED_VERSION}.tgz && docker-php-ext-enable memcached \
&& cd /tmp && pecl install imagick-${IMAGICK_VERSION}.tgz && docker-php-ext-enable imagick \
&& cd /tmp && pecl install mongodb-${MONGODB_VERSION}.tgz && docker-php-ext-enable mongodb \
&& cd /tmp && pecl install swoole-${SWOOLE_VERSION}.tgz && docker-php-ext-enable swoole \
&& cd /tmp && pecl install redis-${REDIS_VERSION}.tgz && docker-php-ext-enable redis \
&& cd /tmp && pecl install geoip-${GEOIP_VERSION}.tgz && docker-php-ext-enable geoip \
&& cd /tmp && pecl install yar-${YAR_VERSION}.tgz && docker-php-ext-enable yar \
&& cd /tmp && pecl install msgpack-${MSGPACK_VERSION}.tgz && docker-php-ext-enable msgpack \
&& cd /tmp && pecl install xdebug-${XDEBUG_VERSION}.tgz && docker-php-ext-enable xdebug \
    && echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_autostart=1" >> /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "xdebug.remote_connect_back=1" >> /usr/local/etc/php/conf.d/xdebug.ini \
&& docker-php-source delete \
&& apk del build-dependencies \
    autoconf \
    file \
    g++ \
    gcc \
    dpkg-dev \
    libc-dev \
    libgcc libstdc++ gmp-dev libxml2-dev zlib-dev \
    pkgconf libtool pcre-dev \
    pkgconfig \
    make m4 perl binutils binutils-libs db \
    pkgconf \
    gettext-dev bzip2-dev \
    re2c freetype-dev jpeg-dev libpng-dev libwebp-dev libjpeg-turbo-dev \
&& ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo "Asia/Shanghai" > /etc/timezone \
&& mkdir ${SITE_WORK_DIR} \
&& setfacl -R -m u:www-data:rwx ${SITE_WORK_DIR} \
&& setfacl -R -m d:u:www-data:rwx ${SITE_WORK_DIR} \
&& composer config -g repo.packagist composer ${COMPOSER_MIRROR} \
&& unset CPHALCON_VERSION \
&& unset MCRYPT_VERSION \
&& unset MEMCACHED_VERSION \
&& unset IMAGICK_VERSION \
&& unset MONGODB_VERSION \
&& unset SWOOLE_VERSION \
&& unset REDIS_VERSION \
&& unset GEOIP_VERSION \
&& unset YAR_VERSION \
&& unset MSGPACK_VERSION \
&& unset XDEBUG_VERSION \
&& unset COMPOSER_MIRROR \
&& rm -rf /var/cache/apk/* /tmp/* /var/tmp /var/www

WORKDIR ${SITE_WORK_DIR}

VOLUME ["${SITE_WORK_DIR}"]

EXPOSE 9000