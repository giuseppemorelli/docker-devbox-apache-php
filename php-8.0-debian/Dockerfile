# install on debian 9 for problems with lib dependency like libcurl3 (not installable on debian 10 -> we have libcurl4)
FROM debian:stretch

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

RUN apt-get -y dist-upgrade

RUN wget -O "/etc/apt/trusted.gpg.d/php.gpg" "https://packages.sury.org/php/apt.gpg" \
    && sh -c 'echo "deb https://packages.sury.org/php/ stretch main" > /etc/apt/sources.list.d/php.list'

RUN apt-get update \
    && apt-get -y install \
    apache2 \
    git \
    php8.0 \
    php8.0-common \
    php8.0-cli \
    php8.0-curl \
    php8.0-dev \
    php8.0-gd \
    php8.0-intl \
    php8.0-mysql \
    php8.0-mbstring \
    php8.0-xml \
    php8.0-xsl \
    php8.0-zip \
    php8.0-xdebug \
    php8.0-soap \
    php8.0-bcmath \
    php8.0-imagick	\
    php8.0-exif	\
    php8.0-opcache	\
    php8.0-bcmath \
    php8.0-ctype \
    php8.0-dom \
    php8.0-iconv \
    php8.0-sockets \
    postfix \
    rsync \
    unzip \
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
COPY php/8.0/mods-available/devbox.ini /etc/php/8.0/apache2/conf.d/00-devbox.ini
COPY php/8.0/mods-available/xdebug-3.ini /etc/php/8.0/apache2/conf.d/90-xdebug-3.ini
COPY php/8.0/mods-available/devbox.ini /etc/php/8.0/cli/conf.d/00-devbox.ini
COPY php/8.0/mods-available/xdebug-3.ini /etc/php/8.0/cli/conf.d/90-xdebug-3.ini

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
