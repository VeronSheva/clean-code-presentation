FROM php:8.2-fpm-alpine

COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/

RUN apk add --no-cache \
		acl \
        bash \
        fcgi \
		file \
		gettext \
		git \
	;

RUN set -eux; \
    install-php-extensions \
    	intl \
    	zip \
    	apcu \
		opcache \
        pdo pdo_mysql \
    ;

#RUN docker-php-ext-install pdo pdo_mysql

ENV COMPOSER_ALLOW_SUPERUSER=1

COPY --from=composer:2.4 /usr/bin/composer /usr/bin/composer

COPY ./app/composer.* ./

RUN composer install --prefer-dist --no-dev --no-scripts --no-progress --no-interaction

COPY ./app .

RUN composer dump-autoload --optimize

RUN curl -sS https://get.symfony.com/cli/installer | bash
RUN mv /root/.symfony*/bin/symfony /usr/local/bin/symfony
