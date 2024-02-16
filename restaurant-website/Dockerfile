# Use an official PHP runtime as a parent image
FROM php:8.3-fpm

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

# Symfony CLI
RUN curl -sS https://get.symfony.com/cli/installer | bash && \
    mv /root/.symfony*/bin/symfony /usr/local/bin/symfony

# Add Symfony to PATH
ENV PATH="/usr/local/bin/symfony:${PATH}"

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

# Switch back to the root user
USER root

# Run Composer dump-autoload to generate the autoloader
RUN composer dump-autoload --optimize

# Expose the port for production
EXPOSE 8000

# Start PHP-FPM when the container runs
CMD ["symfony", "server:start", "--port=8000", "--allow-http", "--no-tls", "--dir=/var/www/html"]
