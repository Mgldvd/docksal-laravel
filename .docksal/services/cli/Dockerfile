FROM docksal/cli:php8.2-3.5

# create the log file and provide permission to the www-data user
RUN touch /tmp/xdebug.log && chown www-data:www-data /tmp/xdebug.log

# same thing for the PHP error log
RUN touch /var/log/php-errors.log && chown www-data:www-data /var/log/php-errors.log

RUN apt update \
    && apt install -y libssl-dev wget git unzip default-mysql-client iputils-ping

RUN pecl apt update \
    && apt install libzip-dev -y \
    && docker-php-ext-install zip \
    && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install mysqli && \
    docker-php-ext-install pdo_mysql

RUN bash -c '[[ -n "$(pecl list | grep mongodb)" ]]\
 || (pecl install mongodb && docker-php-ext-enable mongodb)'

RUN pecl install -o -f redis \
&&  rm -rf /tmp/pear \
&&  docker-php-ext-enable redis \
&&  docker-php-ext-enable pdo_mysql

COPY docker-php.ini /usr/local/etc/php/conf.d/
COPY xdebug.ini /usr/local/etc/php/conf.d/

RUN curl -sS https://getcomposer.org/installer | php

RUN mv composer.phar /usr/local/bin/composer
