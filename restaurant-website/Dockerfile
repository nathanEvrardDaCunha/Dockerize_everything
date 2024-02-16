# Use an official PHP runtime as a parent image
FROM php:8.3-fpm

# Set the APP_ENV environment variable to "prod"
ENV APP_ENV=prod

# Set the working directory in the container
WORKDIR /var/www/html

# Copy all files to the container
COPY . .

# Install dependencies and configure PHP extensions
RUN apt-get update \
    && apt-get install -y libpng-dev libicu-dev zip unzip \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl pdo pdo_mysql gd

# Install Composer globally
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Copy only the necessary files for Composer install
COPY composer.json composer.lock ./

# Set ownership for Composer cache directory
RUN mkdir -p /var/www/.composer/cache/files && \
    chown -R www-data:www-data /var/www/.composer

# Set permissions for log file and directory
RUN touch /var/www/html/var/log/dev.log

# Set permissions for asset_mapper directory
RUN mkdir -p /var/www/html/var/cache/dev/asset_mapper

# Set ownership for all directory to www-data user
RUN chown -R www-data:www-data *

# Install Composer dependencies
USER www-data
RUN composer install --no-dev --optimize-autoloader

# Run Composer update to potentially get the latest versions
RUN composer update

# Clear Composer cache
RUN composer clear-cache

# Switch back to the root user
USER root

# Run Composer dump-autoload to generate the autoloader
RUN composer dump-autoload

# Compile JS and CSS
RUN php bin/console asset-map:compile

# Clear the cache
RUN php bin/console cache:clear --env=prod


# Expose the port for production
EXPOSE 8000

# Start PHP-FPM when the container runs
CMD ["php-fpm"]
