FROM debian:stretch
MAINTAINER tommyZZM

# INSTALL OPENSSL AND NGINX
ENV NGINX_VERSION 1.11.1

RUN apt-get update \
    && apt-get install -y wget unzip openssl build-essential libpcre3 libpcre3-dev zlib1g-dev \
    && wget http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz \
#    && wget -O openssl_1_0_2h.zip -c https://github.com/openssl/openssl/archive/OpenSSL_1_0_2h.zip \
#    && unzip openssl_1_0_2h.zip \
#    && mv openssl-OpenSSL_1_0_2h openssl_1_0_2h \
    && tar -xzvf nginx-${NGINX_VERSION}.tar.gz \
    && cd nginx-${NGINX_VERSION} \
    && ./configure \
        --prefix=/usr/local/nginx \
        --sbin-path=/usr/sbin/nginx \
        --conf-path=/etc/nginx/nginx.conf \
        --pid-path=/var/run/nginx.pid \
        --error-log-path=/var/log/nginx/error.log \
        --http-log-path=/var/log/nginx/access.log \
#        --with-http_ssl_module \
        --with-http_v2_module \
        --with-http_realip_module \
        --with-http_stub_status_module \
        --with-threads \
#        --with-openssl=../openssl_1_0_2h \
        --with-http_addition_module \
        --with-http_gunzip_module \
        --with-http_gzip_static_module \
        --with-stream=dynamic \
#        --with-stream_ssl_module \
        --with-ipv6 \
    && make \
    && make install \
    && make clean \
    && mkdir /etc/nginx/conf.d \
    && mv /etc/nginx/nginx.conf /etc/nginx/conf.d/default.conf \
    && echo "include /etc/nginx/conf.d/*.conf;" >> nginx.conf \
    && apt-get purge build-essential -y \
    && apt-get clean \
    && apt-get autoremove -y \
    && echo "curr location: "$PWD \
    && cd / \
#    && rm -rf openssl_1_0_2h.zip \
#    && rm -rf openssl_1_0_2h \
    && rm -Rf nginx-${NGINX_VERSION}.tar.gz \
    && rm -Rf nginx-${NGINX_VERSION} \
    && rm -Rf /tmp/* \
    && rm -Rf /var/lib/apt/lists/*

EXPOSE 80 8080 443

CMD ["nginx", "-g", "daemon off;"]
