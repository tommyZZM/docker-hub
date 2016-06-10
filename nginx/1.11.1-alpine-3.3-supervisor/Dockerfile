FROM tommyzzm/nginx:1.11.1-alpine-3.3
MAINTAINER tommyZZM

RUN echo "http://dl-4.alpinelinux.org/alpine/edge/testing" >> /etc/apk/repositories \
    && apk --update add python py-pip \
    && python -m pip install --upgrade pip \
    && pip install supervisor \
    && mkdir /var/log/supervisor/ \
    && ln -s /etc/supervisor/supervisord.conf /etc/supervisord.conf \
    && apk --update add incron \
    && apk --update add bash \
    && apk --update add vim \
    && rm -Rf /tmp/* \
    && rm -Rf /var/cache/apk/*

## CONFIG
COPY ./incron.d /etc/incron.d
COPY ./supervisor /etc/supervisor

CMD ["supervisord", "-n", "-c", "/etc/supervisor/supervisord.conf"]
