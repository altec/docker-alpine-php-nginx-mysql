# ./docker/php/Dockerfile
FROM php:7.4.0RC6-fpm-alpine

ENV PHP_OPCACHE_VALIDATE_TIMESTAMPS="0" \
    PHP_OPCACHE_MAX_ACCELERATED_FILES="7963" \
    PHP_OPCACHE_MEMORY_CONSUMPTION="192" \
    PHP_OPCACHE_MAX_WASTED_PERCENTAGE="10"

RUN apk add bash curl zip libzip-dev && rm /var/cache/apk/*
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

RUN docker-php-ext-install pdo_mysql

RUN apk add $PHPIZE_DEPS
RUN pecl install apcu
# RUN pecl install xdebug-2.7.2
RUN docker-php-ext-configure zip
RUN docker-php-ext-install zip opcache
RUN docker-php-ext-enable apcu 
# RUN docker-php-ext-enable xdebug

RUN apk del --purge autoconf g++ make

WORKDIR /var/www

COPY ./docker/php/php.ini /usr/local/etc/php/php.ini
# COPY ./docker/php/xdebug-dev.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
COPY ./docker/php/php-fpm-pool.conf /usr/local/etc/php-fpm.d
COPY ./docker/php/opcache.ini /usr/local/etc/php/conf.d/opcache.ini

COPY ./app/ /var/www

RUN PATH=$PATH:/var/www/bin:bin

CMD ["php-fpm", "-F"]