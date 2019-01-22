FROM redis:5.0

MAINTAINER Oleksandr Storozhenko <ostorozhenko@sharpminds.com>

ARG REDIS_PASSWORD='secret'
ENV REDIS_PASSWORD ${REDIS_PASSWORD}

RUN mkdir -p /usr/local/etc/redis

#####################################
# Copy own conf file
#####################################
COPY redis.conf /usr/local/etc/redis/redis.conf

VOLUME /data

EXPOSE 6379

CMD [ "sh", "-c", "exec redis-server /usr/local/etc/redis/redis.conf --requirepass ${REDIS_PASSWORD}"]