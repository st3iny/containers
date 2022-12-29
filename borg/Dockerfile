FROM debian:bullseye-slim AS builder

ARG BORG_VERSION=1.2.3

RUN apt update \
    && apt install -y curl \
    && cd / \
    && curl -fsSLO https://github.com/borgbackup/borg/releases/download/${BORG_VERSION}/borg-linux64 \
    && chmod +x borg-linux64

FROM debian:bullseye

RUN adduser \
    --disabled-password \
    --disabled-login \
    --uid 2000 \
    borg
COPY --from=builder /borg-linux64 /usr/local/bin/borg

USER borg:borg

ENTRYPOINT ["/usr/local/bin/borg"]
