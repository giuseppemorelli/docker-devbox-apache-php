FROM debian:bullseye

MAINTAINER Giuseppe Morelli <info@giuseppemorelli.net>

VOLUME /var/www/
VOLUME /etc/apache2/sites-enabled/

ENV APACHE_USER_UID     33
ENV APACHE_USER_GID     33
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update \
    && apt-get -y install \
    apache2 \
    git \
    php7.4 \
    php7.4-common \
    php7.4-cli \
    php7.4-curl \
    php7.4-dev \
    php7.4-gd \
    php7.4-intl \
    php7.4-mysql \
    php7.4-mbstring \
    php7.4-xml \
    php7.4-xsl \
    php7.4-zip \
    php7.4-json \
    php7.4-xdebug \
    php7.4-soap \
    php7.4-bcmath \
    php7.4-imagick	\
    php7.4-exif	\
    php7.4-opcache	\
    php7.4-bcmath \
    php7.4-ctype \
    php7.4-dom \
    php7.4-iconv \
    php7.4-sockets \
    postfix \
    rsync \
    unzip \
    wget \
    && apt-get clean \
    && rm -rf \
    /var/lib/apt/lists/* \
    /tmp/* \
    /var/tmp/* \
    /usr/share/man \
    /usr/share/doc \
    /usr/share/doc-base

COPY script /opt/script/
COPY apache2/conf-enabled/* /etc/apache2/conf-enabled/
COPY apache2/sites-enabled/* /etc/apache2/sites-enabled/
COPY php/7.4/mods-available/devbox.ini /etc/php/7.4/apache2/conf.d/00-devbox.ini
COPY php/7.4/mods-available/xdebug-3.ini /etc/php/7.4/apache2/conf.d/90-xdebug-3.ini
COPY php/7.4/mods-available/devbox.ini /etc/php/7.4/cli/conf.d/00-devbox.ini
COPY php/7.4/mods-available/xdebug-3.ini /etc/php/7.4/cli/conf.d/90-xdebug-3.ini

## adding composer 2
RUN cd /tmp/ \
    && wget https://getcomposer.org/composer.phar \
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
