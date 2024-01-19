# Base image
FROM ubuntu:latest

RUN apt-get update && apt-get upgrade -y

RUN apt-get install build-essential libpcre3 libpcre3-dev libssl-dev zlib1g zlib1g-dev libssl-dev -y
RUN apt-get install wget unzip -y

RUN wget http://nginx.org/download/nginx-1.24.0.tar.gz

RUN wget https://github.com/sergey-dryabzhinsky/nginx-rtmp-module/archive/dev.zip

RUN tar -zxvf nginx-1.24.0.tar.gz

RUN unzip dev.zip

WORKDIR ./nginx-1.24.0
#RUN cd ./nginx-1.24.0

RUN ./configure --with-http_ssl_module --add-module=../nginx-rtmp-module-dev
RUN make CFLAGS="-Wno-error=deprecated-declarations"
RUN make
RUN make install

#RUN export PATH=$PATH:/usr/local/nginx/sbin
RUN cp /usr/local/nginx/sbin/nginx /usr/local/sbin/nginx

#COPY ../config/nginx.conf /etc/nginx/nginx.conf
COPY ../config/nginx.conf /usr/local/nginx/conf/nginx.conf

WORKDIR /
#RUN /usr/local/nginx/sbin/nginx

EXPOSE 1935
EXPOSE 80
EXPOSE 8000

COPY ../config/tinycloud-server-start.sh /usr/local/bin
RUN chmod +x /usr/local/bin/tinycloud-server-start.sh


RUN mkdir -p /var/www/html/streams
RUN chown -R www-data:www-data /var/www/html/streams/
#RUN /usr/local/bin/tinycloud-server-start.sh

#CMD ["tail", "-f", "/dev/null"]
#CMD nginx -g daemon off

CMD ["nginx", "-g", "daemon off;"]

#CMD ["/usr/local/nginx/sbin/nginx"]