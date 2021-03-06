FROM alpine:latest

LABEL maintainer="CuCuCumin@gmail.com"

ENV PHPIZE_DEPS \
		autoconf \
		dpkg-dev dpkg \
		file \
		g++ \
		gcc \
		libc-dev \
		make \
		pkgconf \
		re2c

ENV WORK_DIR="/app" \
    COMPOSER_MIRROR="https://packagist.laravel-china.org" \
	PHP_FPM_USER="www-data"

RUN set -ex; \
	sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories \
&& apk --update upgrade --no-cache \
&& apk --update add --no-cache \
    $PHPIZE_DEPS \
    acl \
    ca-certificates \
    curl-dev \
    libwebp \
    libwebp-dev \
    libmemcached-dev \
    zlib-dev \
    geoip-dev \
    imagemagick-dev \
    openssl-dev \
    php7 \
    php7-dev \
    php7-pear \
    php7-phar \
    php7-bcmath \
    php7-bz2 \
    php7-ctype \
    php7-curl \
    php7-calendar \
	php7-dom \
    php7-exif \	
    php7-fpm \
	php7-fileinfo \
    php7-gd \
    php7-gmp \
    php7-iconv \
    php7-intl \
    php7-json \
    php7-mbstring \
    php7-mysqli \
    php7-mysqlnd \
    php7-opcache \
    php7-openssl \
    php7-pcntl \
    php7-pdo \
    php7-pdo_mysql \
    php7-pdo_pgsql \
    php7-pdo_sqlite \
    php7-pgsql \
    php7-posix \
    php7-pecl-event \
    php7-pecl-igbinary \
    php7-pecl-redis \
    php7-pecl-msgpack \
    php7-pecl-mongodb \
    php7-pecl-memcached \
    php7-session \
    php7-soap \
    php7-sockets \
    php7-sysvmsg \
    php7-sysvsem \
    php7-sysvshm \
    php7-sodium \
	php7-simplexml \
    php7-zip \
	php7-zlib \
    php7-tokenizer \
	php7-xml \
	php7-xmlreader \
	php7-xmlwriter \
	composer \
	wget \
&&  wget https://github.com/phalcon/cphalcon/archive/v3.4.3.tar.gz \
&&  tar xvzf v3.4.3.tar.gz \ 
&&  cd cphalcon-3.4.3/build/  \
&&  sh ./install \
&&  echo "extension=phalcon.so" >> /etc/php7/conf.d/phalcon.ini \
&& pecl channel-update pecl.php.net \
&& pecl install yar && echo "extension=yar.so" >> /etc/php7/conf.d/yar.ini \
&& pecl install swoole && echo "extension=swoole.so" >> /etc/php7/conf.d/swoole.ini \
&& pecl install psr && echo "extension=psr.so" >> /etc/php7/conf.d/psr.ini \
&& composer config -g repo.packagist composer ${COMPOSER_MIRROR} \
&& apk del $PHPIZE_DEPS php7-dev \
&& addgroup -g 1000 -S ${PHP_FPM_USER} \
&& adduser -u 1000 -D -S -s /sbin/nologin -G ${PHP_FPM_USER} ${PHP_FPM_USER} \
&& rm -rf /var/cache/apk/* /tmp/* /var/tmp \
&& ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo "Asia/Shanghai" > /etc/timezone \
&& mkdir ${WORK_DIR}



COPY entrypoint.sh /usr/local/bin/
ENTRYPOINT ["entrypoint.sh"]

WORKDIR ${WORK_DIR}

EXPOSE 9000

CMD ["php-fpm7", "-F"]