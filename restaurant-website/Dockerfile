# Use an official PHP runtime as a parent image
FROM php:8.3-fpm

# Set the working directory in the container
WORKDIR /var/www/html

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

# Install Composer dependencies
RUN composer install --no-scripts --no-autoloader

# Copy the rest of the application files
COPY . .

# Run Composer dump-autoload to generate the autoloader
RUN composer dump-autoload --optimize

# Expose the port for production
EXPOSE 80

# Start PHP-FPM when the container runs
CMD ["php-fpm"]
