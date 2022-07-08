FROM php:8.0.14-fpm-buster

# Install system dependencies
RUN apt-get update && apt-get install -y \
    git \
    curl \
    vim \
    libpq-dev \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    zip \
    unzip

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install zip mysqli pdo_pgsql pdo_mysql mbstring exif pcntl bcmath gd

RUN apt-get update

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

COPY docker/php/php.ini $PHP_INI_DIR/php.ini

RUN chown www-data:www-data /var/www/* -R

RUN chmod 775 /var/www/* -R

EXPOSE 9000