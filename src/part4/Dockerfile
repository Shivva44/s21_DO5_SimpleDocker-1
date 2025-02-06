FROM nginx

RUN apt-get update && apt-get install -y gcc spawn-fcgi libfcgi-dev 


COPY ./nginx/nginx.conf /etc/nginx/nginx.conf
COPY ./mini_server.c /etc/mini_server.c

RUN chown -R yormwela:yormwela /usr/bin && \
    chown -R yormwela:yormwela /usr/sbin && \
    chown -R yormwela:yormwela /var && \
    chown -R yormwela:yormwela /run 

USER yormwela

CMD gcc -o /etc/server /etc/mini_server.c -lfcgi && spawn-fcgi -p 8080 /etc/server && nginx -g 'daemon off;'