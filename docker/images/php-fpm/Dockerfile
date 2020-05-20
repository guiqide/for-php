FROM php:7.1-fpm

LABEL maintainer="guiqide <guiqide@gmail.com>" version="1.0"

ENV PHP_OPCACHE_VALIDATE_TIMESTAMPS="0" \
  PHP_OPCACHE_MAX_ACCELERATED_FILES="10000" \
  PHP_OPCACHE_MEMORY_CONSUMPTION="192" \
  PHP_OPCACHE_MAX_WASTED_PERCENTAGE="10"

RUN apt-get update && apt-get install -y \
  && docker-php-ext-install -j$(nproc) bcmath \
  # gd
  && ( \
  apt-get install -y --no-install-recommends \
  libfreetype6-dev \
  libjpeg62-turbo-dev \
  libpng-dev \
  && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
  && docker-php-ext-install -j$(nproc) gd \
  ) \
  # mysqli
  && docker-php-ext-install -j$(nproc) mysqli \
  # pcntl
  && apt-get install -y --no-install-recommends libxml2-dev \
  && docker-php-ext-install -j$(nproc) pcntl \
  # pdo_mysql
  && docker-php-ext-install -j$(nproc) pdo_mysql \
  # soap
  && docker-php-ext-install -j$(nproc) soap \
  # sockets
  && docker-php-ext-install -j$(nproc) sockets \
  # ------ php pecl extension: --------
  # redis
  && pear config-set php_ini /usr/local/etc/php/php.ini-development \
  && pecl install -o -f redis \
  &&  rm -rf /tmp/pear \
  &&  docker-php-ext-enable redis \
  && docker-php-ext-enable redis \
  # zip
  && apt-get install -y --no-install-recommends libzip-dev \
  && curl -fsSL 'https://pecl.php.net/get/zip-1.15.4.tgz' -o zip.tar.gz \
  && mkdir -p zip \
  && tar -xf zip.tar.gz -C zip --strip-components=1 \
  && rm zip.tar.gz \
  && ( \
  cd zip \
  && phpize \
  && ./configure \
  && make -j "$(nproc)" \
  && make install \
  ) \
  && rm -r zip \
  && docker-php-ext-enable zip \
  # clean
  && docker-php-source delete \
  && rm -rf /var/lib/apt/lists/*

COPY ./opcache.ini /usr/local/etc/php/conf.d/opcache.ini