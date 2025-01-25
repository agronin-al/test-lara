FROM php:8.2-fpm
RUN apt-get update && apt-get install -y \
    curl \
    wget \
    libzip-dev \
    zlib1g-dev \
    libxml2-dev \
    zip \
    openssl \
    libfreetype-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    npm \
    libonig-dev \
    && pecl install redis-5.3.7 \
    && pecl install xdebug-3.2.1

RUN docker-php-ext-install \
    pdo \
    pdo_mysql \
#    ctype \
#    mbstring \
#    dom \
#    fileinfo \
#    hash \
#    xml \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-enable redis xdebug

WORKDIR /var/www/laraapp

COPY ./backend ./

RUN chown -R www-data:www-data /var/www/laraapp \
    && chmod -R 775 /var/www/laraapp/storage

COPY --from=composer:2.6.5 /usr/bin/composer /usr/local/bin/composer

COPY ./images/php/docker-php-ext-xdebug.ini /usr/local/etc/php/conf.d/

RUN #composer install && npm install && npm run build

CMD ["php-fpm"]