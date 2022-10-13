FROM nginx:latest

ARG PHP_VERSION=8.0

ENV PHP_VERSION ${PHP_VERSION}

RUN apt-get update && apt-get install apt-transport-https lsb-release ca-certificates -y && \
    apt-get install -y wget && \
    wget -O /etc/apt/trusted.gpg.d/php.gpg https://packages.sury.org/php/apt.gpg && \
    echo "deb https://packages.sury.org/php/ $(lsb_release -sc) main" | tee /etc/apt/sources.list.d/php.list && \
    apt-get update  && apt-get install -y \
    php${PHP_VERSION}-zip php${PHP_VERSION}-dev php${PHP_VERSION}-imagick php${PHP_VERSION}-tidy php${PHP_VERSION}-xmlrpc \
    php${PHP_VERSION}-fpm php${PHP_VERSION}-common php${PHP_VERSION}-gd php${PHP_VERSION}-cli php${PHP_VERSION}-mbstring php${PHP_VERSION}-intl \
    php${PHP_VERSION}-xml php${PHP_VERSION}-opcache php${PHP_VERSION}-mysql php${PHP_VERSION}-curl php${PHP_VERSION}-redis \
    composer

COPY nginx.conf /etc/nginx/
COPY default.template.conf /etc/nginx/default.template
COPY ./logs/. /var/log/nginx/
COPY ./src/. /var/www/html/
COPY ./phpmyadmin/. /var/www/phpmyadmin/
COPY php/pool.d/ /etc/php/${PHP_VERSION}/fpm/pool.d/

RUN cd /var/www/html && composer update

RUN sed -i "s/php8.0-fpm.sock/php${PHP_VERSION}-fpm.sock/g" /etc/php/${PHP_VERSION}/fpm/pool.d/www.conf

RUN sed -i "s/php8.0-fpm.sock/php${PHP_VERSION}-fpm.sock/g" /etc/nginx/default.template

RUN sed -i "s/Php8.0/php${PHP_VERSION}/g" /var/www/html/src/views/index.php

EXPOSE 80

CMD ["/bin/sh", "-c", "/etc/init.d/php${PHP_VERSION}-fpm stop && update-rc.d php${PHP_VERSION}-fpm defaults && /etc/init.d/php${PHP_VERSION}-fpm start && envsubst localhost < /etc/nginx/default.template > /etc/nginx/conf.d/default.conf && nginx -g 'daemon off;'"]
