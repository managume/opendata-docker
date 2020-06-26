FROM php:7.4-apache

ARG DEBIAN_FRONTEND=noninteractive

# Tools
RUN apt-get update \
    && apt-get install -qq -y --no-install-recommends \
    git \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    zlib1g-dev \
    libzip-dev \
    zip \
    unzip \
    curl \
    openssl \
    libonig-dev

# PHP
RUN docker-php-source extract \
    && docker-php-ext-configure \
    mbstring --enable-mbstring \
    && docker-php-ext-install \
    gd \
    pdo_mysql \
    mysqli \
    zip \
    mbstring \
    && docker-php-source delete

# Composer
ENV COMPOSER_HOME /var/www/.composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
RUN mkdir -p $COMPOSER_HOME/cache
ENV PATH  "$PATH:/var/www/html/vendor/bin"

# Drupal
ARG CREATE_DRUPAL_PROJECT
RUN if [ "$CREATE_DRUPAL_PROJECT" = "True" ]; \
    then \
    composer create-project drupal/recommended-project /var/www/html && \
    mkdir -p /var/www/html/web/sites/default/files && \
    cp /var/www/web/sites/example.settings.local.php /var/www/html/web/sites/default/settings.local.php && \
    cp /var/www/web/sites/default/default.settings.php /var/www/html/web/sites/default/settings.php;\
    fi

# Apache
ENV APACHE_DOCUMENT_ROOT /var/www/html/web
RUN sed -ri -e 's!/var/www/html!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/sites-available/*.conf
RUN sed -ri -e 's!/var/www/!${APACHE_DOCUMENT_ROOT}!g' /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

# Permissions
RUN chown -R www-data:www-data /var/www/
RUN chmod -R 755 /var/www
RUN if [ "$CREATE_DRUPAL_PROJECT" = "True" ]; \
    then \
    chmod -R 775 /var/www/html/web/sites/default/files && \
    chmod -R 640 /var/www/html/web/sites/default/settings.php && \
    chmod -R 640 /var/www/html/web/sites/default/settings.local.php; \
    fi