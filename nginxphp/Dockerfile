FROM ubuntu:22.04
LABEL author="srikanth" org="qt" project="LEMP"
ARG DEBIAN_FRONTEND=noninteractive
RUN apt update && \
    apt install nginx -y && \
    apt install  php php-fpm -y 
RUN mkdir /var/www/srapp 
COPY /srapp /etc/nginx/sites-available/srapp
RUN ln -s /etc/nginx/sites-available/srapp /etc/nginx/sites-enabled/ 
RUN unlink /etc/nginx/sites-enabled/default 
RUN service nginx reload
COPY /index.html /var/www/srapp/index.html
COPY /info.php /var/www/srapp/info.php
RUN service nginx reload
EXPOSE 80
#CMD [","nginx", "-g", "daemon off;"]
#CMD /etc/init.d/php7.4-fpm start -F && nginx -g "daemon off;"
CMD ["/bin/bash","-c","service php8.1-fpm restart && nginx -g 'daemon off;'"]
