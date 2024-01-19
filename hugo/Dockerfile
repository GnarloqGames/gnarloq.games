FROM debian:bookworm as build

RUN apt-get update && apt-get install -y curl

RUN curl -L https://github.com/gohugoio/hugo/releases/download/v0.121.2/hugo_extended_0.121.2_linux-amd64.tar.gz -o /tmp/hugo.tar.gz
RUN cd /tmp && tar xzf hugo.tar.gz && cp hugo /bin/hugo && chmod +x /bin/hugo

WORKDIR /build
COPY . .
RUN hugo

FROM caddy:2.7
COPY --from=build /build/public /usr/share/gnarloq
RUN printf ":80\nroot * /usr/share/gnarloq\nfile_server\n" > /config/caddy/Caddyfile
ENTRYPOINT ["caddy", "run", "-c", "/config/caddy/Caddyfile"]