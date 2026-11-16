FROM ghcr.io/meeting-room-booking-system/mrbs:1.12.1

RUN apt-get update && apt-get install -y \
    libpq-dev libapache2-mod-shib \
    && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-install pdo_pgsql

COPY shibboleth2.xml /etc/shibboleth/shibboleth2.xml
COPY metadata-template.xml /etc/shibboleth/metadata-template.xml
COPY metadata-provider.xml /etc/shibboleth/metadata-provider.xml
COPY attribute-map.xml /etc/shibboleth/attribute-map.xml

COPY 000-default.conf /etc/apache2/sites-enabled/000-default.conf
COPY config.inc.php /var/www/html/config.inc.php

ARG MULTIRUN_VERSION=1.1.3
ADD https://github.com/nicolas-van/multirun/releases/download/${MULTIRUN_VERSION}/multirun-x86_64-linux-gnu-${MULTIRUN_VERSION}.tar.gz /tmp
RUN tar -xf /tmp/multirun-x86_64-linux-gnu-${MULTIRUN_VERSION}.tar.gz \
    && mv multirun /bin \
    && rm /tmp/*

RUN mkdir -p /var/run/shibboleth/

CMD ["/bin/multirun", "/usr/local/bin/apache2-foreground", "/usr/sbin/shibd -f -F"]
