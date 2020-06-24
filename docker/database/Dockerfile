FROM mysql:8.0

LABEL maintainer="Alex Storozhenko <a.storozhenko@live.com>"

ARG TZ=UTC
ENV TZ=${TZ}

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN chown -R mysql:mysql /var/lib/mysql/
RUN chmod 0444 /etc/mysql/conf.d/my.cnf

COPY my.cnf /etc/mysql/conf.d/my.cnf

CMD ["mysqld"]

EXPOSE 3306