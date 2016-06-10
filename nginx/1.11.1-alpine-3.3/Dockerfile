FROM alpine:3.3
MAINTAINER tommyZZM

# INSTALL OPENSSL AND NGINX
ENV NGINX_VERSION 1.11.1

RUN apk update && apk upgrade \
    && build_pkgs="perl build-base linux-headers openssl-dev pcre-dev wget zlib-dev" \
    && runtime_pkgs="pcre zlib" \
    && tool_pkgs="wget unzip" \
    && apk add ${build_pkgs} ${runtime_pkgs} ${tool_pkgs}\
    && cd /tmp \
    && wget http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz \
    && wget --no-check-certificate -O openssl_1_0_2h.zip -c https://github.com/openssl/openssl/archive/OpenSSL_1_0_2h.zip \
    && unzip openssl_1_0_2h.zip \
    && mv openssl-OpenSSL_1_0_2h openssl_1_0_2h \
    && tar -xzvf nginx-${NGINX_VERSION}.tar.gz \
    && cd ./nginx-${NGINX_VERSION} \
    && ./configure \
        --prefix=/usr/local/nginx \
        --sbin-path=/usr/sbin/nginx \
        --conf-path=/etc/nginx/nginx.conf \
        --pid-path=/var/run/nginx.pid \
        --error-log-path=/var/log/nginx/error.log \
        --http-log-path=/var/log/nginx/access.log \
        --with-openssl=/tmp/openssl_1_0_2h \
        --with-http_ssl_module \
        --with-http_realip_module \
        --with-http_addition_module \
        --with-http_sub_module \
        --with-http_dav_module \
        --with-http_flv_module \
        --with-http_mp4_module \
        --with-http_gunzip_module \
        --with-http_gzip_static_module \
        --with-http_random_index_module \
        --with-http_secure_link_module \
        --with-http_stub_status_module \
        --with-http_auth_request_module \
        --with-mail \
        --with-mail_ssl_module \
        --with-file-aio \
        --with-ipv6 \
        --with-threads \
        --with-stream \
        --with-stream_ssl_module \
        --with-http_v2_module \
    && make \
    && make install \
    && make clean \
    && mkdir /etc/nginx/conf.d \
    && mv /etc/nginx/nginx.conf /etc/nginx/conf.d/default.conf \
    && touch /etc/nginx/nginx.conf \
    && echo 'include /etc/nginx/conf.d/*.conf;' >> /etc/nginx/nginx.conf \
    && apk del ${build_pkgs} ${tool_pkgs} \
    && cd / \
    && rm -Rf /tmp/* \
    && rm -Rf /var/cache/apk/*

# forward request and error logs to docker log collector
RUN ln -sf /dev/stdout /var/log/nginx/access.log
RUN ln -sf /dev/stderr /var/log/nginx/error.log

EXPOSE 80 8080 443

VOLUME [" /etc/nginx/conf.d"]

CMD ["nginx", "-g", "daemon off;"]
