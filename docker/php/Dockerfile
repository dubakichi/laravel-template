FROM php:8.0-fpm

COPY --from=composer /usr/bin/composer /usr/bin/composer
#RUN cd /usr/bin && curl -s http://getcomposer.org/installer | php && ln -s /usr/bin/composer.phar /usr/bin/composer

RUN apt-get update && apt-get install -y \
   vim \
   zip \
   unzip \
   iputils-ping \
   git \
   libpq-dev \
&& docker-php-ext-install pdo_mysql

WORKDIR /var/www/html
