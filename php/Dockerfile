FROM php:8.0-fpm-alpine
RUN apk update; \
    apk upgrade;
RUN docker-php-ext-install mysqli pdo pdo_mysql

# Install bash
RUN apk add --update bash && rm -rf /var/cache/apk/*

# Use custom php.ini
COPY php.ini "$PHP_INI_DIR/php.ini"