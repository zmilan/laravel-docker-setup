FROM php:8.2-apache

ENV APACHE_DOCUMENT_ROOT /var/www/html/{{PROJECT_NAME}}/public

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

RUN apt-get update -y
RUN apt-get install -y git zip unzip zlib1g-dev libpng-dev libjpeg-dev libfreetype6-dev
RUN pecl install xdebug-3.2.2 redis \
    && docker-php-ext-enable xdebug && docker-php-ext-enable redis
RUN docker-php-ext-install pdo_mysql mysqli gd && docker-php-ext-configure gd --with-freetype --with-jpeg
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer creates=/usr/local/bin/composer
COPY xdebug.ini /etc/php/8.2/mods-available/xdebug.ini
RUN a2enmod rewrite

RUN curl -sL https://deb.nodesource.com/setup_18.x | bash - \
    && apt-get install -y nodejs

ARG UID=$UID
ARG GID=$GROUP_ID

RUN groupadd -g "${GID}" laravel \
  && useradd --create-home --no-log-init -u "${UID}" -g "${GID}" laravel

USER laravel