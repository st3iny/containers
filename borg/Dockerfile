FROM debian:trixie AS builder

ARG BORG_VERSION=1.4.2

# Use german apt mirror
RUN sed -Ei 's|deb.debian.org|ftp.de.debian.org|g' /etc/apt/sources.list.d/*

# https://borgbackup.readthedocs.io/en/stable/installation.html#from-source
RUN apt-get update \
    && apt-get install -y python3 python3-dev python3-pip python3-virtualenv \
        libacl1-dev libacl1 \
        libssl-dev \
        liblz4-dev libzstd-dev libxxhash-dev \
        build-essential \
        pkg-config python3-pkgconfig

RUN virtualenv --python=python3 /borg-venv \
    && . /borg-venv/bin/activate \
    && pip install pkgconfig \
    && pip install borgbackup==${BORG_VERSION}

FROM debian:trixie-slim

COPY --from=builder /borg-venv /borg-venv

RUN sed -Ei 's|deb.debian.org|ftp.de.debian.org|g' /etc/apt/sources.list.d/* \
    && apt-get update \
    && apt-get install -y python3 \
        libacl1 \
        libssl3 \
        liblz4-1 libzstd1 libxxhash0 \
    && apt-get clean \
    && useradd \
        --uid 2000 \
        --user-group \
        --create-home \
        borg

USER borg:borg

ENTRYPOINT ["/borg-venv/bin/borg"]
