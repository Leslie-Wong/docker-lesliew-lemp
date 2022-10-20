FROM mysql:5.7

ENV MYSQL_DATABASE my_db
ENV MYSQL_ALLOW_EMPTY_PASSWORD yes

# COPY dbdata /var/lib/mysql
COPY conf.d/ /etc/mysql/conf.d

# RUN /bin/sh -c /etc/init.d/php${PHP_VERSION}-fpm stop && update-rc.d php${PHP_VERSION}-fpm defaults && /etc/init.d/php${PHP_VERSION}-fpm start && envsubst localhost < /etc/nginx/default.template > /etc/nginx/conf.d/default.conf && nginx -g "daemon off;"

EXPOSE 3306

# CMD ["/bin/sh", "-c", "/etc/init.d/php${PHP_VERSION}-fpm stop && update-rc.d php${PHP_VERSION}-fpm defaults && /etc/init.d/php${PHP_VERSION}-fpm start && envsubst localhost < /etc/nginx/default.template > /etc/nginx/conf.d/default.conf && nginx -g 'daemon off;'"]