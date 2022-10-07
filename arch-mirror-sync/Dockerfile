FROM alpine:3.16

RUN apk add --no-cache rsync rclone

COPY entrypoint /entrypoint
RUN chmod +x /entrypoint

ENTRYPOINT ["/entrypoint"]
