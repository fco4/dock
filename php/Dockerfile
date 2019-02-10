FROM php:7.2-fpm

RUN apt-get update && apt-get install -y \
        curl \
        wget \
        git \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
    && docker-php-ext-install -j$(nproc) iconv \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

RUN apt-get -y install libz-dev libmemcached-dev libmemcached11 libmemcachedutil2 build-essential \
        && pecl install memcached-2.2.0 \
        && echo extension=memcached.so >> /usr/local/etc/php/conf.d/memcached.ini \
        && apt-get remove -y build-essential libmemcached-dev libz-dev \
        && apt-get autoremove -y \
        && apt-get clean \
        && rm -rf /tmp/pear


RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

ADD php.ini /usr/local/etc/php/conf.d/40-custom.ini

WORKDIR /var/www/noc1

CMD ["php-fpm"]