FROM php:8.2-apache

ENV APACHE_DOCUMENT_ROOT /var/www/{{PROJECT_NAME}}/public

RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

RUN apt-get update -y
RUN apt-get install -y \
    git \
    zip \
    unzip \
    zlib1g-dev \
    libgd-dev \
    libicu-dev \
    libsodium-dev \
    ca-certificates curl gnupg \
    libfreetype-dev \
    libjpeg62-turbo-dev \
    libpng-dev
# configure intl extension
RUN docker-php-ext-configure intl
# configure and install gd extension
RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    	&& docker-php-ext-install -j$(nproc) gd

RUN pecl install xdebug-3.2.2 redis \
    && docker-php-ext-enable xdebug && docker-php-ext-enable redis

RUN docker-php-ext-install pdo_mysql mysqli bcmath exif intl sodium

# install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer creates=/usr/local/bin/composer
# configure xdebug
COPY xdebug.ini /etc/php/8.2/mods-available/xdebug.ini
# enable apache mode rewrite
RUN a2enmod rewrite

# install node
RUN mkdir -p /etc/apt/keyrings && curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
RUN apt-get update && apt-get install nodejs -y

# args from .env file
ARG USER_ID
ARG GROUP_ID

RUN groupadd -g "${GROUP_ID}" laravel \
  && useradd --create-home --no-log-init -u "${USER_ID}" -g "${GROUP_ID}" laravel

USER laravel