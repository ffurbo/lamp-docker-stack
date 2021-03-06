FROM php:7.3-apache

RUN apt-get update \
    && apt-get -y install \
        unzip \
        libjpeg-dev \
        libpng-dev \
        libfreetype6-dev \
        libicu-dev \
        libmagickwand-dev \
        git \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

#        gnupg2 && \
#    apt-key update && \
#    apt-get update && \
#    apt-get -y install \
#            default-mysql-client \
#        --no-install-recommends && \
#        apt-get clean && \
#        rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*


# Add configuration files
COPY image-files/ /

RUN docker-php-ext-configure gd \
        --with-freetype-dir=/usr/include/ \
        --with-png-dir=/usr/include/ \
        --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-install \
        pdo_mysql \
        gd \
        intl


# Install PECL extensions
# see http://stackoverflow.com/a/8154466/291573) for usage of `printf`
RUN printf "\n" | pecl install \
        imagick \
        memcache && \
    docker-php-ext-enable \
        imagick \
        memcache

#RUN mkdir -p /app/web/
#RUN echo "<?php phpinfo();">/app/web/phpinfo.php
#RUN echo "<?php phpinfo();">/var/www/html/phpinfo.php

# Enable mod_rewrite for images with apache
RUN if command -v a2enmod >/dev/null 2>&1; then \
        a2enmod rewrite headers \
    ;fi

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- \
        --filename=composer.phar \
        --install-dir=/usr/local/bin && \
    composer.phar clear-cache