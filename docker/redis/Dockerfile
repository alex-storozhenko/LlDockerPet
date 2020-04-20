FROM redis:5.0

LABEL maintainer="Alex Storozhenko <a.storozhenko@live.com>"

ARG REDIS_PASSWORD='secret'

RUN mkdir -p /usr/local/etc/redis

#####################################
# Copy own conf file
#####################################
COPY redis.conf /usr/local/etc/redis/redis.conf

VOLUME /data

EXPOSE 6379

ENV REDIS_PASSWORD=${REDIS_PASSWORD}

CMD [ "sh", "-c", "exec redis-server /usr/local/etc/redis/redis.conf --requirepass ${REDIS_PASSWORD}"]