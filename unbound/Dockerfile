FROM alpine:3.8

MAINTAINER Sergey Ershov "ershovu@gmail.com"

RUN apk add --update --no-cache unbound ca-certificates openssl bash  \
  && unbound-anchor -a /etc/unbound/root.key ; true \
  && unbound-control-setup \
  && wget ftp://FTP.INTERNIC.NET/domain/named.cache -O /etc/unbound/root.hints \
  && apk del openssl \
  && rm -rf /var/cache/apk/* ;

COPY unbound.conf /etc/unbound/unbound.conf

#ENTRYPOINT ["/usr/sbin/unbound", "-dd"]
ENTRYPOINT ["/usr/sbin/unbound", "-d"]
