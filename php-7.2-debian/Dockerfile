FROM ubuntu:18.04

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
    wget

RUN apt-get -y update \
    && apt-get -y install \
    apache2 \
    git \
    php-dev \
    libmcrypt-dev \
    php-pear \
    php7.2 \
    php7.2-cli \
    php7.2-curl \
    php7.2-dev \
    php7.2-gd \
    php7.2-intl \
    php7.2-yaml \
    php7.2-mysql \
    php7.2-mbstring \
    php7.2-xml \
    php7.2-xsl \
    php7.2-zip \
    php7.2-json \
    php7.2-xdebug \
    php7.2-soap \
    php7.2-bcmath \
    postfix \
    unzip \
    && apt-get clean \
    && rm -rf \
    /var/lib/apt/lists/* \
    /tmp/* \
    /var/tmp/* \
    /usr/share/man \
    /usr/share/doc \
    /usr/share/doc-base

RUN pecl channel-update pecl.php.net
RUN pecl install mcrypt

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
