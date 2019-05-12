FROM php:7.2-fpm

RUN apt-get update && apt-get install -y --no-install-recommends \
        git \
        zlib1g-dev \
        libxml2-dev \
        libzip-dev \
        unzip \
        libbz2-dev \
        bzip2 \
        imagemagick \
        libwebp-dev \
        libjpeg-dev \
        libpng-dev \
        libz-dev \
        libxpm-dev \
        libgmp-dev \
        libmagickwand-dev \
        libmagickcore-dev \
    && docker-php-ext-install \
        pdo_mysql \
        mysqli \
        zip \
        bcmath \
        bz2 \
        calendar \
        dba \
        exif \
        gd \
        gettext \
        gmp \
        intl

RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer

RUN pecl install imagick apcu xdebug opcache \
    && docker-php-ext-enable imagick \
    && docker-php-ext-enable apcu \
    && docker-php-ext-enable xdebug \
    && docker-php-ext-enable opcache

ARG APCU_INI=/usr/local/etc/php/conf.d/docker-php-ext-apcu.ini
ARG XDEBUG_INI=/usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

RUN echo "xdebug.default_enable = on" >> ${XDEBUG_INI} \
    && echo "xdebug.remote_enable = on" >> ${XDEBUG_INI} \
    && echo "xdebug.remote_autostart = off" >> ${XDEBUG_INI} \
    && echo "xdebug.remote_connect_back = off" >> ${XDEBUG_INI} \
    && echo "xdebug.remote_port = 9000" >> ${XDEBUG_INI} \
    && echo "xdebug.remote_host = 10.254.254.254" >> ${XDEBUG_INI} \
    && echo "xdebug.profiler_enable_trigger = on" >> ${XDEBUG_INI} \
    && echo "xdebug.profiler_output_name = xdebug-profile-cachegrind.out-%H-%R" >> ${XDEBUG_INI} \
    && echo "xdebug.var_display_max_children = 128" >> ${XDEBUG_INI} \
    && echo "xdebug.var_display_max_data = 512" >> ${XDEBUG_INI} \
    && echo "xdebug.var_display_max_depth = 3" >> ${XDEBUG_INI}

RUN echo "apc.enabled = on" >> ${APCU_INI} \
    && echo "apc.shm_size = 64M" >> ${APCU_INI} \
    && echo "apc.ttl = 7200" >> ${APCU_INI} \
    && echo "apc.enable_cli = on" >> ${APCU_INI}

RUN echo "date.timezone = Europe/Zurich" > /usr/local/etc/php/conf.d/timezone.ini
RUN echo 'memory_limit = 2G' >> /usr/local/etc/php/conf.d/docker-php-memlimit.ini

COPY . /var/www/sites
WORKDIR /var/www/sites/
