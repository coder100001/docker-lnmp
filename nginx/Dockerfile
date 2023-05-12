ARG OPENRESTY_VERSION
FROM openresty/openresty:${OPENRESTY_VERSION}

RUN apt update

RUN mkdir -p /usr/local && mkdir -p /var/lib/nginx \
    && mkdir -p /usr/local/lib
RUN mkdir -p /usr/share && mkdir -p /usr/local/nginx \
    && mkdir -p /usr/share/nginx/html && chmod -R 777 /usr/share/nginx/html \
    && touch /usr/share/nginx/html/50x.html
    
