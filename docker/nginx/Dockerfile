# ./docker/php/Dockerfile
FROM nginx:1.16-alpine

ENV USER=www-data
ENV UID=12345
ENV GID=23456

RUN adduser \
    --disabled-password \
    --gecos "" \
    --home "$(pwd)" \
    --ingroup "$USER" \
    --no-create-home \
    --uid "$UID" \
    "$USER"

RUN rm /etc/nginx/conf.d/default.conf

COPY ./docker/nginx/nginx.conf /etc/nginx/nginx.conf
COPY ./docker/nginx/nginxconfig.io/ /etc/nginx/nginxconfig.io
COPY ./docker/nginx/sites-available/ /etc/nginx/sites-available

# Tell docker that all future commands should run as the appuser user
# USER www-data