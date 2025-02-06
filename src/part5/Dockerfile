FROM nginx
WORKDIR /home
COPY ./nginx/nginx.conf /etc/nginx/nginx.conf
COPY mini_server.c .
RUN apt-get update && \
    apt-get install -y gcc libfcgi-dev spawn-fcgi &&\
    rm -rf /var/lib/apt/lists/* 
RUN mkdir -p /var/cache/nginx /var/log/nginx /etc/nginx/conf.d && \
    chown -R nginx:nginx /home /var/cache/nginx /var/log/nginx /etc/nginx/conf.d /var/lib/dpkg && \
    chmod -R 755 /home
RUN touch /var/run/nginx.pid && \
    chown -R nginx:nginx /var/run/nginx.pid
USER nginx
CMD gcc -o server mini_server.c -lfcgi && spawn-fcgi -p 8080 server && nginx -g 'daemon off;'



