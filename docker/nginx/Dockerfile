FROM nginx:alpine

LABEL maintainer="Alex Storozhenko <a.storozhenko@live.com>"

COPY nginx.conf /etc/nginx/

ARG PUID=1000
ENV PUID ${PUID}

RUN apk update \
    && apk upgrade \
    && apk add --no-cache openssl \
    && apk add --no-cache bash \
    && adduser -D -H -u ${PUID} -s /bin/bash www-data -G www-data

ARG PHP_UPSTREAM_CONTAINER=phpfpm
ARG PHP_UPSTREAM_PORT=9000

# Set upstream conf and remove the default conf
RUN echo "upstream php-upstream { server ${PHP_UPSTREAM_CONTAINER}:${PHP_UPSTREAM_PORT}; }" > /etc/nginx/conf.d/upstream.conf \
    && rm /etc/nginx/conf.d/default.conf

ADD ./startup.sh /opt/startup.sh
RUN sed -i 's/\r//g' /opt/startup.sh
CMD ["/bin/bash", "/opt/startup.sh"]

EXPOSE 80 443