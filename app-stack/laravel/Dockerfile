FROM php:8.2-apache

RUN apt-get update && apt-get install -y \
     git curl zip unzip libpng-dev libonig-dev libxml2-dev libzip-dev default-mysql-client \
     && docker-php-ext-install pdo_mysql mysqli mbstring exif pcntl bcmath gd zip \
     && a2enmod rewrite \
     && rm -rf /var/lib/apt/lists/*

COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

EXPOSE 80

CMD ["apache2-foreground"]
