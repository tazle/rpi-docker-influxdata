FROM resin/rpi-raspbian:stretch

RUN for server in $(shuf -e ha.pool.sks-keyservers.net \
                            hkp://p80.pool.sks-keyservers.net:80 \
                            keyserver.ubuntu.com \
                            hkp://keyserver.ubuntu.com:80 \
                            pgp.mit.edu) ; do \
        gpg --keyserver "$server" --recv-keys 05CE15085FC09D18E99EFB22684A14CF2582E0C5 && break || : ; \
    done

ENV INFLUXDB_VERSION 1.4.3
RUN curl -O https://dl.influxdata.com/influxdb/releases/influxdb_${INFLUXDB_VERSION}_armhf.deb.asc && \
    curl -O https://dl.influxdata.com/influxdb/releases/influxdb_${INFLUXDB_VERSION}_armhf.deb && \
    gpg --batch --verify influxdb_${INFLUXDB_VERSION}_armhf.deb.asc influxdb_${INFLUXDB_VERSION}_armhf.deb && \
    dpkg -i influxdb_${INFLUXDB_VERSION}_armhf.deb && \
    rm -f influxdb_${INFLUXDB_VERSION}_armhf.deb*
COPY influxdb.conf /etc/influxdb/influxdb.conf

EXPOSE 8086

VOLUME /var/lib/influxdb

COPY entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["influxd"]
