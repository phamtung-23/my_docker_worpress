FROM php:7.3-fpm-buster

RUN apt-get update && apt-get install -y \
    cron \
    git \
    gzip \
    libbz2-dev \
    libfreetype6-dev \
    libicu-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libonig-dev \
    libpng-dev \
    libsodium-dev \
    libssh2-1-dev \
    libxslt1-dev \
    libzip-dev \
    lsof \
    default-mysql-client \
    vim \
    zip \
    procps \
    wget

RUN docker-php-ext-configure \
    gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/

RUN docker-php-ext-install \
    bcmath \
    bz2 \
    calendar \
    exif \
    gd \
    gettext \
    intl \
    mbstring \
    mysqli \
    opcache \
    pcntl \
    pdo_mysql \
    soap \
    sockets \
    sodium \
    sysvmsg \
    sysvsem \
    sysvshm \
    xsl \
    msmtp \
    mailutils \
    zip

ARG XDEBUG_VERSION=""
RUN pecl install xdebug${XDEBUG_VERSION} \
    && docker-php-ext-enable xdebug

RUN pecl install ssh2-1.2 \
    && docker-php-ext-enable ssh2

ARG UID=1000
ARG GID=1000
ARG XDEBUG_PORT=9003
RUN groupadd -g ${GID} app || true \
    && useradd -g "$(getent group $GID | cut -d: -f1)" -u ${UID} -d /var/www -s /bin/bash app

COPY conf/msmtprc /etc/msmtprc

COPY conf/www.conf /usr/local/etc/php-fpm.d/
COPY conf/php.ini /usr/local/etc/php/

ENV PATH="${PATH}:/var/www/html"

RUN echo "\nxdebug.client_port = ${XDEBUG_PORT}" | tee -a /usr/local/etc/php/php.ini

RUN sed -i -e "s/listen\.group\s*=\s*app/listen.group = $(getent group $GID | cut -d: -f1)/g" /usr/local/etc/php-fpm.conf

RUN mkdir -p /etc/nginx/html /var/www/html /sock \
    && chown -R app /etc/nginx /var/www /usr/local/etc/php/conf.d /sock

USER app

VOLUME /var/www

WORKDIR /var/www/html

EXPOSE 9000



