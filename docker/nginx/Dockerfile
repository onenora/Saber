FROM alpine:3.16
LABEL maintainer="altriabot@gmail.com"

ARG NGINX_VERSION=1.22.0

RUN \
  build_pkgs="build-base linux-headers openssl-dev pcre-dev wget zlib-dev" && \
  runtime_pkgs="ca-certificates openssl pcre zlib tzdata git" && \
  apk --no-cache add ${build_pkgs} ${runtime_pkgs} && \
  cd /tmp && \
  wget https://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz && \
  tar xzf nginx-${NGINX_VERSION}.tar.gz && \
  cd /tmp/nginx-${NGINX_VERSION} && \
  ./configure \
    --prefix=/etc/nginx \
    --sbin-path=/usr/sbin/nginx \
    --conf-path=/etc/nginx/nginx.conf \
    --error-log-path=/var/log/nginx/error.log \
    --http-log-path=/var/log/nginx/access.log \
    --pid-path=/var/run/nginx.pid \
    --lock-path=/var/run/nginx.lock \
    --http-client-body-temp-path=/var/cache/nginx/client_temp \
    --user=nginx \
    --group=nginx \
    --with-http_ssl_module \
    --with-threads \
    --with-ipv6 \
    --with-http_v2_module \
    --with-http_realip_module \
    --with-http_stub_status_module \
    --with-http_sub_module \
    --with-stream \
    --with-stream_realip_module \
    --with-stream_ssl_module \
    --with-stream_ssl_preread_module && \
  make && \
  make install && \
  addgroup -S nginx && \
  adduser -D -S -h /var/cache/nginx -s /sbin/nologin -G nginx nginx && \
  rm -rf /tmp/* && \
  apk del ${build_pkgs} && \
  rm -rf /var/cache/apk/* && \
  mkdir -p /usr/share/nginx/html && \
  mkdir /etc/nginx/stream.conf.d  && \
  mkdir /etc/nginx/http.conf.d && \
  mkdir /etc/nginx/ssl && \
  mkdir /var/run/nginx && \
  touch /var/run/nginx/nginx.pid && \
  cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && \
  echo "Asia/Shanghai" > /etc/timezone

ADD nginx.conf /etc/nginx/nginx.conf

VOLUME ["/var/cache/nginx"]

EXPOSE 80 443

CMD ["nginx", "-g", "daemon off;"]
