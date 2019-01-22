FROM php:7.3-fpm-alpine

LABEL maintainer="Alex Storozhenko <a.storozhenko@live.com>"

ARG PUID=1000
ENV PUID ${PUID}
ARG TZ=UTC
ENV TZ ${TZ}
ARG INSTALL_XDEBUG=false
ENV INSTALL_XDEBUG = ${INSTALL_XDEBUG}
ARG INSTALL_OPCACHE=false
ENV INSTALL_OPCACHE ${INSTALL_OPCACHE}

#####################################
# Php ext`s install
#####################################
RUN apk add --no-cache \
            freetype \
            libpng \
            libjpeg-turbo \
            freetype-dev \
            libpng-dev \
            libjpeg-turbo-dev \
            zlib-dev \
            zip \
            unzip \
            libzip-dev \
            hiredis-dev \
            openssl \
            bash \
            shadow

RUN docker-php-ext-install iconv \
        && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ \
                                       --with-jpeg-dir=/usr/include/ \
                                       --with-png-dir=/usr/include/ \
        && docker-php-ext-install gd \
        && docker-php-ext-install pdo \
        && docker-php-ext-install pdo_mysql \
        && docker-php-ext-configure zip --with-libzip \
        && docker-php-ext-install zip \
        && docker-php-ext-install tokenizer \
        && docker-php-ext-install mbstring

#####################################
# Phpredis extension install
#####################################
ENV PHPREDIS_VERSION 4.2.0

RUN mkdir -p /usr/src/php/ext/redis \
        && curl -L https://github.com/phpredis/phpredis/archive/${PHPREDIS_VERSION}.tar.gz | tar xvz -C /usr/src/php/ext/redis --strip 1 \
        && echo 'redis' >> /usr/src/php-available-exts \
        && docker-php-ext-install redis

#####################################
# Phpiredis extension install
#####################################
ENV PHPIREDIS_VERSION 1.0.0

RUN mkdir -p /usr/src/php/ext/phpiredis \
        && curl -L https://github.com/nrk/phpiredis/archive/v${PHPIREDIS_VERSION}.tar.gz | tar xvz -C /usr/src/php/ext/phpiredis --strip 1 \
        && echo 'phpiredis' >> /usr/src/php-available-exts \
        && docker-php-ext-install phpiredis

#####################################
# Xdebug install
#####################################


RUN if [ ${INSTALL_XDEBUG} = true ]; then \
    apk add --no-cache $PHPIZE_DEPS \
        && pecl install xdebug \
        && docker-php-ext-enable xdebug \
;fi

# Copy xdebug configuration for remote debugging
COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

RUN sed -i "s/xdebug.remote_autostart=0/xdebug.remote_autostart=1/" /usr/local/etc/php/conf.d/xdebug.ini && \
    sed -i "s/xdebug.remote_enable=0/xdebug.remote_enable=1/" /usr/local/etc/php/conf.d/xdebug.ini && \
    sed -i "s/xdebug.cli_color=0/xdebug.cli_color=1/" /usr/local/etc/php/conf.d/xdebug.ini

###########################################################################
# Opcache:
###########################################################################
ARG INSTALL_OPCACHE=false

RUN if [ ${INSTALL_OPCACHE} = true ]; then \
    docker-php-ext-install opcache \
;fi

# Copy opcache configration
COPY ./opcache.ini /usr/local/etc/php/conf.d/opcache.ini

#####################################
# Non-Root User
#####################################
RUN usermod -u ${PUID} www-data

#####################################
# Set Timezone
#####################################
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

#
#--------------------------------------------------------------------------
# Final Touch
#--------------------------------------------------------------------------
#
COPY ./laravel-lumen.ini /usr/local/etc/php/conf.d
COPY ./xlaravel-lumen.pool.conf /usr/local/etc/php-fpm.d/

WORKDIR /var/www/html