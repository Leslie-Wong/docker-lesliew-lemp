FROM ubuntu:latest

ARG PHP_VERSION=8.1

ENV TZ=Asia/hong_kong \
    DEBIAN_FRONTEND=noninteractive

ENV PHP_VERSION ${PHP_VERSION}

RUN apt-get update -y && apt-get install -y software-properties-common && add-apt-repository ppa:ondrej/php

RUN apt-get install apt-transport-https lsb-release ca-certificates -y

RUN apt-get update && apt-get install -y wget nginx php${PHP_VERSION} \
    php${PHP_VERSION}-zip php${PHP_VERSION}-dev php${PHP_VERSION}-imagick php${PHP_VERSION}-tidy php${PHP_VERSION}-xmlrpc \
    php${PHP_VERSION}-fpm php${PHP_VERSION}-common php${PHP_VERSION}-gd php${PHP_VERSION}-cli php${PHP_VERSION}-mbstring php${PHP_VERSION}-intl \
    php${PHP_VERSION}-xml php${PHP_VERSION}-opcache php${PHP_VERSION}-mysql php${PHP_VERSION}-curl php${PHP_VERSION}-redis \
    composer

RUN phpv=`echo ${PHP_VERSION} | sed s/[.]//g` && if test  ${phpv} -ge 72 ; then cd /var/www \
    && wget -c "https://files.phpmyadmin.net/phpMyAdmin/5.2.0/phpMyAdmin-5.2.0-all-languages.tar.gz" \
    && tar -zxvf phpMyAdmin-5.2.0-all-languages.tar.gz && mv phpMyAdmin-5.2.0-all-languages phpmyadmin ; \ 
    else cd /var/www \
    && wget -c "https://files.phpmyadmin.net/phpMyAdmin/4.9.10/phpMyAdmin-4.9.10-all-languages.tar.gz" \
    && tar -zxvf phpMyAdmin-4.9.10-all-languages.tar.gz && mv phpMyAdmin-4.9.10-all-languages phpmyadmin ; \ 
    fi


COPY ./phpmyadmin/config.inc.php /var/www/phpmyadmin/config.inc.php

COPY nginx.conf /etc/nginx/
COPY default.template.conf /etc/nginx/default.template
COPY ./logs/. /var/log/nginx/
COPY ./src/. /var/www/html/
COPY php/pool.d/ /etc/php/${PHP_VERSION}/fpm/pool.d/

RUN sed -i "s/php8.0-fpm.sock/php${PHP_VERSION}-fpm.sock/g" /etc/php/${PHP_VERSION}/fpm/pool.d/www.conf

RUN sed -i "s/php8.0-fpm.sock/php${PHP_VERSION}-fpm.sock/g" /etc/nginx/default.template

RUN sed -i "s/Php8.0/php${PHP_VERSION}/g" /var/www/html/src/views/index.php

EXPOSE 80

CMD ["/bin/sh", "-c", "/etc/init.d/php${PHP_VERSION}-fpm stop && update-rc.d php${PHP_VERSION}-fpm defaults && /etc/init.d/php${PHP_VERSION}-fpm start && envsubst localhost < /etc/nginx/default.template > /etc/nginx/conf.d/default.conf && nginx -g 'daemon off;'"]
