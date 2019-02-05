# ./docker/php/Dockerfile
FROM php:7.3-fpm-alpine

RUN apk add curl zip nano && rm /var/cache/apk/*
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

RUN docker-php-ext-install pdo_mysql

RUN apk add $PHPIZE_DEPS
RUN pecl install apcu
RUN docker-php-ext-enable apcu 

WORKDIR /usr/src/app

COPY ./apps/supermare /usr/src/app

RUN PATH=$PATH:/usr/src/apps/vendor/bin:bin

CMD ["php-fpm", "-F"]