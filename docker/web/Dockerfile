FROM php:8.0-apache
COPY php.ini /usr/local/etc/php/
COPY apache2.conf /etc/apache2/apache2.conf
COPY --from=composer /usr/bin/composer /usr/bin/composer
COPY keys /etc/apache2/ssl/keys
COPY default-ssl.conf /etc/apache2/sites-available/default-ssl.conf
RUN apt-get update \
  && apt-get install -y zlib1g-dev default-mysql-client libzip-dev\
  && apt-get install -y libicu-dev \
  && apt-get install -y vim \
  && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
  && docker-php-ext-install zip pdo_mysql mysqli intl \
  && docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ \
  && docker-php-ext-install -j$(nproc) gd

RUN a2enmod ssl
RUN a2ensite default-ssl
RUN service apache2 restart
RUN composer self-update --2

# apacheのrewriteを有効にする
RUN cd /etc/apache2/mods-enabled \
    && ln -s ../mods-available/rewrite.load

RUN apt install python3-pip -y \
  && pip3 install awscli --upgrade --user\
  && apt install awscli -y
  # && aws configure -AKIATREDYGSDI46DNFNT -TarIFJoPs8WCG3I0RKgaBH+OnZAPjB5TlXJNY2v6 -ap-northeast-1 -text \
  # && aws s3 ls

WORKDIR /var/www/html
