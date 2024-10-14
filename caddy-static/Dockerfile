FROM caddy:latest

RUN mkdir /static

COPY ./Caddyfile /etc/caddy/Caddyfile
COPY ./index.html /static/index.html

ENTRYPOINT ["caddy"]
CMD ["run", "--config", "/etc/caddy/Caddyfile", "--adapter", "caddyfile"]
