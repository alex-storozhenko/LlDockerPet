FROM php:7.3-cli-stretch

LABEL maintainer="Alex Storozhenko <a.storozhenko@live.com>"

ARG TZ=UTC
ARG PUID=1000
ARG PGID=1000
ARG PHPREDIS_VERSION=5.2.1

RUN DEBIAN_FRONTEND=noninteractive

#####################################
# Php ext`s install
#####################################
RUN apt-get update && apt-get install -y \
            libpng-dev \
            libfreetype6-dev \
            libjpeg62-turbo-dev \
            libpng-dev \
            libicu-dev \
            g++ \
            unixodbc-dev \
            libxml2-dev \
            libaio-dev \
            freetds-dev \
            libssl-dev \
            libhiredis-dev \
            openssl \
            libzip-dev \
            zip \
            unzip \
                && apt-get -y autoremove \
                && apt-get clean \
                && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

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
ENV PHPREDIS_VERSION=${PHPREDIS_VERSION}

RUN mkdir -p /usr/src/php/ext/redis \
        && curl -L https://github.com/phpredis/phpredis/archive/${PHPREDIS_VERSION}.tar.gz | tar xvz -C /usr/src/php/ext/redis --strip 1 \
        && echo 'redis' >> /usr/src/php-available-exts \
        && docker-php-ext-install redis

#####################################
# Install tools and additional soft
#####################################
RUN apt-get update && apt-get install -y \
            git \
            htop \
            mc \
                && apt-get -y autoremove \
                && apt-get clean \
                && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

#####################################
# Composer install
#####################################
RUN php -r "readfile('http://getcomposer.org/installer');" \
    | php -- --install-dir=/usr/bin/ --filename=composer

VOLUME /app

#####################################
# Non-Root User
#####################################
ENV PUID=${PUID}
ENV PGID=${PGID}

RUN groupadd -g ${PGID} app && \
    useradd -u ${PUID} -g app -m app

RUN chown -R app:app /home/app

#####################################
# Set Timezone
#####################################
ENV TZ=${TZ}

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

USER app

ENTRYPOINT ["php", "-a"]

WORKDIR /home/app