FROM debian:jessie

MAINTAINER michimau <mauro.michielon@eea.europa.eu>

RUN apt-get -y update && apt-get install -y \
        vim \
        apache2 \
        php5 \
        php5-mysql \
        php5-gd \
        libapache2-mod-php5

COPY default /etc/apache2/sites-available/000-default.conf

RUN a2enmod php5 ldap rewrite

EXPOSE 80

CMD /usr/sbin/apache2ctl -D FOREGROUND
