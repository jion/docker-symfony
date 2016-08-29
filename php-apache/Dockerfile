FROM php:5.6-apache

MAINTAINER Manuel Schnidrig <el.manu@gmail.com>

# Manually set up the apache environment variables
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid

# Extra tools
RUN DEBIAN_FRONTEND=noninteractive && apt-get update && apt-get upgrade -qq -y && apt-get install -qq -y \
    git \
    curl \
    zip \
    php5-cli \
    zlib1g-dev \
    libicu-dev \
    g++ \
    sudo

# Apache mods.
RUN a2enmod rewrite
RUN docker-php-ext-configure intl
RUN docker-php-ext-install pdo_mysql intl

RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

COPY php.ini /usr/local/etc/php/
COPY console /usr/local/bin/console

# Update the default apache site with the config we created.
ADD apache-config.conf /etc/apache2/sites-enabled/000-default.conf

WORKDIR /var/www/symfony

# RUN usermod -u 1000 www-data
# RUN chown -R www-data:www-data /var/www/symfony/app/cache
# RUN chown -R www-data:www-data /var/www/symfony/app/logs