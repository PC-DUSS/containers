FROM docker.io/library/ubuntu:22.04

RUN apt update && DEBIAN_FRONTEND=noninteractive TZ=America/Toronto apt-get -y install tzdata
RUN apt install php8.1-fpm apache2 --no-install-recommends -y
RUN echo "ProxyPassMatch ^/(.*\.php(/.*)?)$ fcgi://127.0.0.1:9000/var/www/html/\$1" >> /etc/apache2/sites-available/000-default.conf
RUN echo "DirectoryIndex /index.php index.php index.html" >> /etc/apache2/sites-available/000-default.conf
COPY ./config_files/www.conf /etc/php/8.1/fpm/pool.d/www.conf
#RUN sed -i 's#listen[[:blank:]]*=[[:blank:]]*/var/run/php/.*#listen[[:blank:]]=[[:blank:]]9000#g' /etc/php/8.1/fpm/pool.d/www.conf
RUN a2dismod mpm_prefork
RUN a2enmod mpm_event
RUN a2enmod proxy
RUN a2enmod proxy_fcgi
RUN service apache2 stop
RUN service php8.1-fpm start

#CMD ["apache2ctl", "-D", "FOREGROUND"]
CMD service php8.1-fpm start;apache2ctl -DFOREGROUND

EXPOSE 80
