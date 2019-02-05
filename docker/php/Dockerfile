# ./docker/php/Dockerfile
FROM php:7.3-fpm-alpine

RUN apk --update add curl zip && rm /var/cache/apk/*
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

RUN docker-php-ext-install pdo_mysql

RUN apk add --no-cache \
    $PHPIZE_DEPS
RUN pecl install apcu
RUN docker-php-ext-enable apcu 

WORKDIR /usr/src/app

COPY --chown=nobody ./apps/supermare /usr/src/app

RUN PATH=$PATH:/usr/src/apps/vendor/bin:bin

CMD ["php-fpm", "-F"]