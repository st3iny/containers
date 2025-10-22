FROM debian:trixie AS builder

ARG TARGETARCH

ARG BORG_VERSION=1.4.1
ARG RCLONE_VERSION=1.71.2

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

RUN apt-get install -y wget unzip \
    && wget -O /rclone.zip \
        https://github.com/rclone/rclone/releases/download/v${RCLONE_VERSION}/rclone-v${RCLONE_VERSION}-linux-${TARGETARCH}.zip \
    && unzip -j /rclone.zip '*/rclone' \
    && chmod +x /rclone

FROM debian:trixie-slim

COPY --from=builder /borg-venv /borg-venv
COPY --from=builder /rclone /usr/local/bin/rclone

RUN sed -Ei 's|deb.debian.org|ftp.de.debian.org|g' /etc/apt/sources.list.d/* \
    && apt-get update \
    \
    # Install borg
    && apt-get install -y python3 \
        libacl1 \
        libssl3 \
        liblz4-1 libzstd1 libxxhash0 \
    && ln -s /borg-venv/bin/borg /usr/local/bin/borg \
    \
    # Clean up
    && apt-get clean \
    \
    # Add custom user
    && useradd \
        --uid 2000 \
        --user-group \
        --create-home \
        borg

USER borg:borg
WORKDIR /home/borg

ENTRYPOINT ["/usr/bin/bash"]
