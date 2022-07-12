FROM php:7.2-apache-stretch

MAINTAINER Giuseppe Morelli <info@giuseppemorelli.net>

VOLUME /var/www/
VOLUME /etc/apache2/sites-enabled/

ENV APACHE_USER_UID     33
ENV APACHE_USER_GID     33
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get -y update \
    && apt-get -y install \
    apt-transport-https \
    ca-certificates \
    wget curl

RUN apt-get -y update \
    && apt-get -y install \
    apache2 \
    git \
    postfix \
    unzip \
    libxml2 \
    libxml2-dev \
    zip \
    libpng-dev \
    zlib1g-dev \
    libzip-dev \
    libxslt-dev \
    libmcrypt-dev \
    mcrypt \
    && apt-get clean \
    && rm -rf \
    /var/lib/apt/lists/* \
    /tmp/* \
    /var/tmp/* \
    /usr/share/man \
    /usr/share/doc \
    /usr/share/doc-base

RUN docker-php-ext-install bcmath soap json mbstring intl gd xsl intl pdo_mysql mysqli xml
RUN docker-php-ext-configure zip --with-libzip
RUN docker-php-ext-install zip

RUN pecl install mcrypt
RUN docker-php-ext-enable mcrypt


COPY script /opt/script/
COPY apache2/conf-enabled/* /etc/apache2/conf-enabled/
COPY apache2/sites-enabled/* /etc/apache2/sites-enabled/
COPY php/7.2/mods-available/devbox.ini /etc/php/7.2/apache2/conf.d/00-devbox.ini
COPY php/7.2/mods-available/xdebug.ini /etc/php/7.2/apache2/conf.d/90-xdebug.ini
COPY php/7.2/mods-available/devbox.ini /etc/php/7.2/cli/conf.d/00-devbox.ini
COPY php/7.2/mods-available/xdebug.ini /etc/php/7.2/cli/conf.d/90-xdebug.ini

## adding composer 2
RUN cd /tmp/ \
    && wget https://getcomposer.org/download/latest-2.2.x/composer.phar \
    && chmod +x ./composer.phar \
    && mv ./composer.phar /usr/local/bin/composer \
    && composer self-update

RUN a2enmod rewrite \
    && a2enmod vhost_alias \
    && a2enmod ssl

ENV \
  POSTFIX_myhostname=hostname \
  POSTFIX_mydestination=localhost \
  POSTFIX_mynetworks=0.0.0.0/0 \
  POSTFIX_relayhost=''

EXPOSE 80
EXPOSE 443
CMD ["/opt/script/entrypoint.sh"]
