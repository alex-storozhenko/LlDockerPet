FROM php:7.3-cli-alpine

LABEL maintainer="Alex Storozhenko <a.storozhenko@live.com>"

ARG TZ=UTC
ENV TZ ${TZ}

# Add a non-root user:
ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}

RUN addgroup -g ${PGID} worker && \
    adduser -D -G worker -u ${PUID} worker

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

###########################################################################
# Opcache:
###########################################################################
RUN docker-php-ext-install opcache

# Copy opcache configration
COPY ./opcache.ini /usr/local/etc/php/conf.d/opcache.ini

#####################################
# Install tools and additional soft
#####################################
RUN apk add --no-cache \
              supervisor

#####################################
# Set Timezone
#####################################
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

#
#--------------------------------------------------------------------------
# Optional Supervisord Configuration
#--------------------------------------------------------------------------
#
# Modify the ./supervisor.conf file to match your App's requirements.
# Make sure you rebuild your container with every change.
#

COPY supervisord.conf /etc/supervisord.conf

ENTRYPOINT ["/usr/bin/supervisord", "-n", "-c",  "/etc/supervisord.conf"]

WORKDIR /etc/supervisor/conf.d/