FROM debian:bullseye

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
    && sh -c 'echo "deb https://packages.sury.org/php/ bullseye main" > /etc/apt/sources.list.d/php.list'

RUN apt-get install gnupg2 -y

RUN apt-key adv --fetch-keys https://packages.sury.org/php/apt.gpg

RUN apt-get update \
    && apt-get -y install --no-install-recommends \
    apt-utils \
    apache2 \
    git \
    php8.2 \
    php8.2-common \
    php8.2-cli \
    php8.2-curl \
    php8.2-dev \
    php8.2-gd \
    php8.2-intl \
    php8.2-mysql \
    php8.2-mbstring \
    php8.2-xml \
    php8.2-xsl \
    php8.2-zip \
    php8.2-xdebug \
    php8.2-soap \
    php8.2-bcmath \
    php8.2-imagick	\
    php8.2-exif	\
    php8.2-opcache	\
    php8.2-bcmath \
    php8.2-ctype \
    php8.2-dom \
    php8.2-iconv \
    php8.2-sockets \
    php8.2-dev \
    php8.2-tokenizer \
    php8.2-phpdbg \
    postfix \
    rsync \
    unzip \
    ca-certificates

RUN apt-get clean \
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
COPY php/mods-available/devbox.ini /etc/php/8.2/apache2/conf.d/00-devbox.ini
COPY php/mods-available/xdebug-3.ini /etc/php/8.2/apache2/conf.d/90-xdebug-3.ini
COPY php/mods-available/devbox.ini /etc/php/8.2/cli/conf.d/00-devbox.ini
COPY php/mods-available/xdebug-3.ini /etc/php/8.2/cli/conf.d/90-xdebug-3.ini

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
