FROM ubuntu:16.04

LABEL MAINTAINER="LiYiMing"

RUN apt-get update && apt-get install -y locales \
  && locale-gen en_US.UTF-8

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

RUN apt-get update \
  && apt-get install -y nginx curl zip unzip git software-properties-common supervisor sqlite3 \
  && add-apt-repository -y ppa:ondrej/php \
  && apt-get update \
  && apt-get install -y php7.2-fpm php7.2-cli php7.2-gd php7.2-mysql \
    php7.2-imap php7.2-memcached php7.2-mbstring php7.2-xml php7.2-curl \
    php7.2-sqlite3 \
  && php -r "copy('https://install.phpcomposer.com/installer', 'composer-setup.php');" \
  && php composer-setup.php \
  && php -r "unlink('composer-setup.php');" \
  && mv composer.phar /usr/local/bin/composer \
  && mkdir /run/php \
  && apt-get remove -y --purge software-properties-common \
  && apt-get -y autoremove \
  && apt-get clean \
  && rm -rf /var/lib/apt/list/* /tmp/* /var/tmp/* \
  && echo "daemon off;" >> /etc/nginx/nginx.conf \
  && ln -sf /dev/stdout /var/log/nginx/access.log \
  && ln -sf /dev/stderr /var/log/nginx/error.log

COPY default /etc/nginx/sites-available/default
COPY php-fpm.conf /etc/php/7.2/fpm/php-fpm.conf

EXPOSE 80

COPY supervisord.conf /etc/supervisor/conf.d/supervisor.conf

CMD "/usr/bin/supervisord"