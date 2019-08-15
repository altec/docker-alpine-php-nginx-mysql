# ./docker/php/Dockerfile
FROM php:7.3-fpm-alpine

RUN apk add bash curl zip libzip-dev && rm /var/cache/apk/*
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

RUN docker-php-ext-install pdo_mysql

RUN apk add $PHPIZE_DEPS
RUN pecl install apcu
RUN docker-php-ext-configure zip --with-libzip
RUN docker-php-ext-install zip
RUN docker-php-ext-enable apcu zip

WORKDIR /var/www

COPY ./app/ /var/www

RUN PATH=$PATH:/var/www/bin:bin

CMD ["php-fpm", "-F"]