FROM alpine:3.15

RUN adduser \
    --disabled-password \
    --uid 2000 \
    borg
RUN apk add --no-cache borgbackup

VOLUME /cache

ENTRYPOINT ["borg"]
