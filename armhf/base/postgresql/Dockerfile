FROM zsoltm/debian-armhf:jessie

ENV PG_MAJOR 9.4
ENV LANG en_US.UTF-8
ENV PATH /usr/lib/postgresql/${PG_MAJOR}/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
ENV PGDATA /var/lib/postgresql/data

## Prerequisites

RUN apt-get update && apt-get install -y --no-install-recommends wget locales postgresql-common\
 && sed -ri 's/#(create_main_cluster) .*$/\1 = false/' /etc/postgresql-common/createcluster.conf\
 && apt-get install -y --no-install-recommends postgresql-${PG_MAJOR} postgresql-contrib\
 && apt-get clean && rm -rf /var/lib/apt/lists/*\
 && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8

# grab gosu for easy step-down from root
ENV GOSU_VERSION 1.7
RUN set -x \
	&& wget -O /usr/local/bin/gosu "http://qnap.dorowu.com/gitlab/uploads/walker/binary/gosu_${GOSU_VERSION}-$(dpkg --print-architecture)" \
	&& chmod +x /usr/local/bin/gosu \
	&& gosu nobody true

VOLUME /var/lib/postgresql/data
COPY entrypoint.sh /
ENTRYPOINT ["/entrypoint.sh"]
EXPOSE 5432
CMD ["postgres"]
