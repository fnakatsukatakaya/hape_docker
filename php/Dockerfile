FROM php:7.3.29-apache

# composerのインストール
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# 各種必要なライブラリのインストール

RUN apt-get update \
    && apt-get install -y \
                zlib1g-dev \
                libzip-dev \
                mariadb-client \
                git \
                zip \
                libfreetype6-dev \ 
                libjpeg62-turbo-dev \
                libpng-dev \
                libjpeg-dev \
                npm \
                vim \
    && docker-php-ext-install \
                pdo_mysql \
                bcmath \
                gd \
                zip \
    && docker-php-ext-configure gd \
                --with-freetype-dir=/usr/include/ \
                --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

# xdebug
RUN pecl install xdebug-3.1.6 && docker-php-ext-enable xdebug

# Configuration.
ADD ./php.ini /usr/local/etc/php/conf.d/php.ini
ADD ./000-default.conf /etc/apache2/sites-enabled/000-default.conf

RUN a2enmod rewrite