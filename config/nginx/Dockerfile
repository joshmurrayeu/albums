FROM nginx:alpine

COPY conf.d/ /etc/nginx/conf.d
RUN rm -f /etc/nginx/conf.d/default.conf
RUN mkdir -p /var/www/albums-api/public
RUN touch /var/www/albums-api/public/index.php