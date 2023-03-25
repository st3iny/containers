FROM debian:bullseye-slim

ARG BORG_VERSION=1.2.4

# https://borgbackup.readthedocs.io/en/stable/installation.html#from-source
RUN apt-get update \
    && apt-get install -y python3 python3-dev python3-pip \
        libacl1-dev libacl1 \
        libssl-dev \
        liblz4-dev libzstd-dev libxxhash-dev \
        build-essential \
        pkg-config python3-pkgconfig \
    && pip install borgbackup==${BORG_VERSION}

RUN adduser \
    --disabled-password \
    --disabled-login \
    --uid 2000 \
    borg

USER borg:borg

ENTRYPOINT ["borg"]
