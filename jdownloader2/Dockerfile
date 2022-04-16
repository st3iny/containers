FROM openjdk:8-slim-bullseye

COPY init /init
COPY usr/local/bin /usr/local/bin

RUN    mkdir -p /etc/jdownloader /opt/jdownloader \
    && chown -R root:root /usr/local/bin/* /init \
    && chown -R 1000:1000 /opt/jdownloader \
    && chmod 0700 /opt/jdownloader \
    && chmod +x /usr/local/bin/* \
    && apt-get update \
    && apt-get dist-upgrade -y \
    && apt-get install -y jq \
    && rm -r /var/lib/apt/lists/*

VOLUME /etc/jdownloader
VOLUME /opt/jdownloader

USER 1000:1000

ENTRYPOINT ["/usr/local/bin/start-jdownloader"]
