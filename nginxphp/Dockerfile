FROM ubuntu:22.04
LABEL author="srikanth" org="qt" project="LAMP"
ARG DEBIAN_FRONTEND=noninteractive
RUN apt update && \
    apt install vim -y && \
    apt install nginx -y && \
    apt install php-fpm -y 
WORKDIR /var/www/html
COPY /info.php /var/www/html/info.php
EXPOSE 80
CMD [ "nginxctl","-D","FOREGROUND" ]
