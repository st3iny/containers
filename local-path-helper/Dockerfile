FROM debian:trixie-slim

RUN apt-get update \
    && apt-get install -y btrfs-progs \
    && rm -rf /var/lib/apt/lists/*
