# Use PHP 8.2 with FPM from Debian Bullseye as base image
FROM php:8.2-fpm-bullseye

# Install system dependencies and security updates
RUN apt-get update && apt-get upgrade -y && apt-get install -y \
    git \
    curl \
    zip \
    unzip \
    libzip-dev \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libcurl4-openssl-dev \
    libssl-dev \
    libpq-dev \
    && docker-php-ext-install pdo pdo_mysql zip mbstring exif pcntl bcmath \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Set the working directory
WORKDIR /var/www

# Copy the Laravel application files
COPY . .

# Install Laravel dependencies
RUN composer install --no-interaction --optimize-autoloader

# Change ownership of the files for PHP-FPM
RUN chown -R www-data:www-data /var/www

# Expose port 9000 for PHP-FPM
EXPOSE 9000

# Start PHP-FPM
CMD ["php-fpm"]
