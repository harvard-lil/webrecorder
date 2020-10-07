FROM nginx:1.15

RUN apt-get update && apt-get install -y openssl

COPY 404.html /usr/share/nginx/html/404.html
COPY 50x.html /usr/share/nginx/html/50x.html
