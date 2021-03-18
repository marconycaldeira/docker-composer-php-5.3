FROM ubuntu:12.04

VOLUME ["/var/www"]

RUN apt-get update && \
    apt-get install -y \
      apache2 \
      apache2-dev \
      php5 \
      php5-cli \
      libapache2-mod-php5 \
      php5-gd \
      php5-ldap \
      php5-mysql \
      php5-pgsql \
      libcurl3  \
      libcurl3-dev \
      php5-curl \
      curl \
      make \
      php-pear \
      php5-dev

RUN CFLAGS="-fPIC" && OPENSSL_VERSION="1.0.2d" \
      && cd /tmp \
      && mkdir openssl \
      && curl -sL "https://www.openssl.org/source/openssl-$OPENSSL_VERSION.tar.gz" -o openssl.tar.gz \
      && curl -sL "https://www.openssl.org/source/openssl-$OPENSSL_VERSION.tar.gz.asc" -o openssl.tar.gz.asc \
      && tar -xzf openssl.tar.gz -C openssl --strip-components=1 \
      && cd /tmp/openssl \
      && ./config shared && make && make install \
      && rm -rf /tmp/*

RUN apt-get install -y libpng12-dev libjpeg-dev libpq-dev zip
RUN pecl install -o -f xdebug-2.2.7

COPY apache_default /etc/apache2/sites-available/default
COPY run /usr/local/bin/run
RUN chmod +x /usr/local/bin/run
RUN a2enmod rewrite
RUN echo "extension=curl.so" >> /etc/php5/apache2/php.ini
RUN echo "zend_extension=xdebug.so" >> /etc/php5/apache2/php.ini

EXPOSE 80
CMD ["/usr/local/bin/run"]
