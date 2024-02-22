# Use an official PHP runtime as a parent image
FROM php:8.3-fpm-alpine

# Set the APP_ENV environment variable to "prod"
ENV APP_ENV=prod

# Set the working directory in the container
WORKDIR /var/www/html

# Install required packages using apk
RUN apk update && \
    apk add --no-cache libpng-dev libjpeg-turbo-dev libwebp-dev zlib-dev libxpm-dev icu-dev zip unzip && \
    docker-php-ext-configure intl && \
    docker-php-ext-install intl pdo pdo_mysql gd

# Install Composer globally
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Copy all files to the container
COPY . .

ENV COMPOSER_ALLOW_SUPERUSER=1

RUN composer install --no-dev --optimize-autoloader && composer clear-cache

# TODO: Improve ownership management
RUN chmod -R 777 var public

# Switch back to the root user //
USER www-data:www-data

# Compile JS and CSS
RUN php bin/console asset-map:compile --env=prod

# Clear the cache
RUN php bin/console cache:clear --env=prod
