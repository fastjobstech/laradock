FROM webdevops/apache:ubuntu-18.04

LABEL maintainer="Eric Pfeiffer <computerfr33k@users.noreply.github.com>"

ARG PHP_UPSTREAM_CONTAINER=php-fpm
ARG PHP_UPSTREAM_PORT=9000
ARG PHP_UPSTREAM_TIMEOUT=60
ARG DOCUMENT_ROOT=/var/www/

ENV WEB_PHP_SOCKET=${PHP_UPSTREAM_CONTAINER}:${PHP_UPSTREAM_PORT}

ENV WEB_DOCUMENT_ROOT=${DOCUMENT_ROOT}

ENV WEB_PHP_TIMEOUT=${PHP_UPSTREAM_TIMEOUT}

EXPOSE 80 443

WORKDIR /var/www/

COPY vhost.conf /etc/apache2/sites-enabled/vhost.conf

# install openssl
RUN apt-get install -y openssl

# create a folder for the keys
RUN mkdir /etc/apache2/ssl 2> /dev/null

# generate the keys for your local domain
COPY openssl.conf /etc/apache2/ssl/openssl.conf
RUN openssl req -days 365 -x509 -out /etc/apache2/ssl/localhost.crt -keyout /etc/apache2/ssl/localhost.key \
  -newkey rsa:2048 -nodes -sha256 \
  -subj '/CN=*.fastjobs.fun' -extensions EXT -config /etc/apache2/ssl/openssl.conf

ENTRYPOINT ["/opt/docker/bin/entrypoint.sh"]

CMD ["supervisord"]
