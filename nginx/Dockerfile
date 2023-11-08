FROM debian:bullseye-slim

RUN export DEBIAN_FRONTEND=noninteractive \
    && apt-get update \
    && apt-get -y install --no-install-recommends \
    ca-certificates \
    curl \
    gettext-base \
    less \
    nginx \
    && ln -sf /dev/null /var/log/nginx/access.log \
    && ln -sf /dev/stdout /var/log/nginx/access_tornator.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log \
    && apt-get -yq autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

EXPOSE 80

ENTRYPOINT ["docker-entrypoint.sh"]

CMD ["/usr/sbin/nginx" "-g" "daemon off"]