FROM lscr.io/linuxserver/bookstack:23.10.1

ARG PUID=1000
ARG PGID=1000
ARG APP_URL
ARG APP_KEY
ARG DB_HOST
ARG DB_DATABASE
ARG DB_USER
ARG DB_PASS
ARG MAIL_DRIVER
ARG MAIL_HOST
ARG MAIL_PORT
ARG MAIL_USERNAME
ARG MAIL_PASSWORD
ARG MAIL_ENCRYPTION

ENV PUID=${PUID} \
    PGID=${PGID} \
    APP_URL=${APP_URL} \
    APP_KEY=${APP_KEY} \
    DB_HOST=${DB_HOST} \
    DB_DATABASE=${DB_DATABASE} \
    DB_USER=${DB_USER} \
    DB_PASS=${DB_PASS} \
    MAIL_DRIVER=${MAIL_DRIVER} \
    MAIL_HOST=${MAIL_HOST} \
    MAIL_PORT=${MAIL_PORT} \
    MAIL_USERNAME=${MAIL_USERNAME} \
    MAIL_PASSWORD=${MAIL_PASSWORD} \
    MAIL_ENCRYPTION=${MAIL_ENCRYPTION}

RUN set -eux; \
  if [ -z "${PUID}" ] || [ -z "${PGID}" ]; then \
    echo "ERROR: PUID and PGID must be set" >&2; exit 1; \
  fi; \
  CURRENT_UID=$(id -u abc); \
  CURRENT_GID=$(id -g abc); \
  if [ "${CURRENT_GID}" != "${PGID}" ]; then \
    groupmod -g "${PGID}" abc || echo "Warning: groupmod failed, GID may already exist"; \
  fi; \
  if [ "${CURRENT_UID}" != "${PUID}" ]; then \
    usermod -u "${PUID}" abc || echo "Warning: usermod failed, UID may already exist"; \
  fi; \
  chown -R abc:abc /config

COPY nginx.conf /etc/nginx/nginx.conf

HEALTHCHECK --interval=30s --timeout=10s --retries=3 \
  CMD curl -f http://localhost/ || exit 1

EXPOSE 80
CMD ["/init"]
