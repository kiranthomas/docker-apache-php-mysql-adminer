FROM php:5.6-apache

ARG USE_XDEBUG=no
ENV COMPOSER_VERSION 1.0.0-alpha11

RUN apt-get update \
  && apt-get install -y libicu-dev php5-mysql \
  && docker-php-ext-install mysqli intl mbstring pdo pdo_mysql \
  && a2enmod rewrite

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer --version=${COMPOSER_VERSION}

RUN set -ex \
	&& if [ $USE_XDEBUG = "yes" ]; then \
        pecl install xdebug && docker-php-ext-enable xdebug; \
        echo "xdebug.remote_enable=on" >> /usr/local/etc/php/conf.d/xdebug.ini; \
        echo "xdebug.remote_autostart=off" >> /usr/local/etc/php/conf.d/xdebug.ini; \
        echo "xdebug.remote_connect_back=on" >> /usr/local/etc/php/conf.d/xdebug.ini; \
        echo "xdebug.remote_port=9090" >> /usr/local/etc/php/conf.d/xdebug.ini; \
	fi

ADD php.ini /usr/local/etc/php/php.ini

EXPOSE 8080

ADD entrypoint.sh /usr/local/bin/entrypoint.sh
RUN chmod +x /usr/local/bin/entrypoint.sh

WORKDIR /var/www/html

ENTRYPOINT ["entrypoint.sh"]
