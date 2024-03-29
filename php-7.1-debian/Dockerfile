FROM php:7.1-apache-buster

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
    wget \
    rsync

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

RUN chmod +x /usr/local/bin/install-php-extensions && \
    install-php-extensions zip gd intl mcrypt yaml xsl soap bcmath mysqli pdo_mysql

#RUN wget -O "/etc/apt/trusted.gpg.d/php.gpg" "https://packages.sury.org/php/apt.gpg" \
#    && sh -c 'echo "deb https://packages.sury.org/php/ stretch main" > /etc/apt/sources.list.d/php.list'

RUN apt-get -y update \
    && apt-get -y install \
    apache2 \
    git \
#    php7.1 \
#    php7.1-cli \
#    php7.1-curl \
#    php7.1-dev \
#    php7.1-gd \
#    php7.1-intl \
#    php7.1-mcrypt \
#    php7.1-yaml \
#    php7.1-mysql \
#    php7.1-mbstring \
#    php7.1-xml \
#    php7.1-xsl \
#    php7.1-zip \
#    php7.1-json \
#    php7.1-xdebug \
#    php7.1-soap \
#    php7.1-bcmath \
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

COPY script /opt/script/
COPY apache2/conf-enabled/* /etc/apache2/conf-enabled/
COPY apache2/sites-enabled/* /etc/apache2/sites-enabled/
COPY php/7.1/mods-available/devbox.ini /usr/local/etc/php/00-devbox.ini
COPY php/7.1/mods-available/xdebug.ini /usr/local/etc/php/90-xdebug.ini
#COPY php/7.1/mods-available/devbox.ini /etc/php/7.1/cli/conf.d/00-devbox.ini
#COPY php/7.1/mods-available/xdebug.ini /etc/php/7.1/cli/conf.d/90-xdebug.ini

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
