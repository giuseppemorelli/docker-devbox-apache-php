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
    php8.3 \
    php8.3-common \
    php8.3-cli \
    php8.3-curl \
    php8.3-dev \
    php8.3-gd \
    php8.3-intl \
    php8.3-mysql \
    php8.3-mbstring \
    php8.3-xml \
    php8.3-xsl \
    php8.3-zip \
    php8.3-xdebug \
    php8.3-soap \
    php8.3-bcmath \
    php8.3-imagick	\
    php8.3-exif	\
    php8.3-opcache	\
    php8.3-bcmath \
    php8.3-ctype \
    php8.3-dom \
    php8.3-iconv \
    php8.3-sockets \
    php8.3-dev \
    php8.3-tokenizer \
    php8.3-phpdbg \
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
COPY php/mods-available/devbox.ini /etc/php/8.3/apache2/conf.d/00-devbox.ini
COPY php/mods-available/xdebug-3.ini /etc/php/8.3/apache2/conf.d/90-xdebug-3.ini
COPY php/mods-available/devbox.ini /etc/php/8.3/cli/conf.d/00-devbox.ini
COPY php/mods-available/xdebug-3.ini /etc/php/8.3/cli/conf.d/90-xdebug-3.ini

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
