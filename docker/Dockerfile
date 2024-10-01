# use PHP 8.2
FROM php:8.3-fpm

# Install common php extension dependencies
RUN apt-get update && apt-get install -y \
    libfreetype-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    zlib1g-dev \
    libzip-dev \
    unzip \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install zip \
    && docker-php-ext-install pdo pdo_mysql  # Add this line to install PDO MySQL

# Change PHP memory limit to 1GB
RUN sed -i 's/^memory_limit = .*/memory_limit = 1024M/' /etc/php8/php.ini

# Set the working directory
COPY ./src /var/www/html
WORKDIR /var/www/html

RUN chown -R www-data:www-data /var/www/html \
    && chmod -R 775 /var/www/html/storage \
    && chmod -R 775 /var/www/html/bootstrap/cache

# install composer
COPY --from=composer:2.6.5 /usr/bin/composer /usr/local/bin/composer

# copy composer.json to workdir & install dependencies
RUN composer install

# Set the default command to run php-fpm
CMD ["php-fpm"]
