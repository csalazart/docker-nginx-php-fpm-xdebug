FROM php:7.1-fpm

ENV UUID=1000
ENV GUID=120
ENV TIMEZONE=Europe/Madrid

MAINTAINER Koreander <csalazart33@gmail.com>

#RUN useradd -ms /bin/bash www-data
RUN usermod -u $UUID www-data
RUN groupmod -g $GUID www-data

RUN apt-get update

RUN apt-get install -y git wget curl unzip zip zlib1g-dev libpq-dev git libicu-dev libxml2-dev apt-utils

# install xdebug
RUN pecl install xdebug-2.6.1
RUN docker-php-ext-enable xdebug
RUN echo "zend_extension=/usr/local/lib/php/extensions/no-debug-non-zts-20160303/xdebug.so" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "error_reporting = E_ALL" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "display_startup_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "display_errors = On" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

# relevant to this answer
RUN echo "xdebug.idekey=\"PHPSTORM\"" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_port=9000" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_enable=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_autostart=1" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
RUN echo "xdebug.remote_host=docker.for.win.localhost" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

RUN echo 'alias sf="php bin/console"' >> ~/.bashrc


RUN curl --insecure https://getcomposer.org/composer.phar -o /usr/bin/composer && chmod +x /usr/bin/composer
RUN composer --version

# Set timezone
RUN rm /etc/localtime
#RUN ln -s /usr/share/zoneinfo/Europe/Madrid /etc/localtime
#RUN "date"

RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone
RUN printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini
RUN "date"

RUN mkdir -p /var/www/var/logs

RUN chown www-data:www-data /var/www -Rf
RUN chmod 776 /var/www -Rf

USER www-data

WORKDIR /var/www