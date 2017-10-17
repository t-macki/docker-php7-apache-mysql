FROM php:7.1-apache

# Install libraries
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
      libicu-dev \
      libmagickwand-dev \
    && rm -r /var/lib/apt/lists/*

# Install PHP extensions
RUN docker-php-ext-install \
      intl \
      pcntl \
      zip \
      opcache \
      pdo_mysql \
      mysqli \
    && pecl install xdebug imagick \
    && docker-php-ext-enable xdebug imagick


# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer

# Put apache config
COPY apache-php-application.conf /etc/apache2/sites-available/php-application.conf
RUN a2ensite php-application.conf
RUN a2dissite 000-default.conf
RUN a2enmod rewrite

# Change uid and gid of apache to docker user uid/gid
RUN usermod -u 1000 www-data && groupmod -g 1000 www-data

WORKDIR /var/www/html
