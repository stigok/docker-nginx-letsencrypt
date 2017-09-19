FROM alpine
MAINTAINER stig+docker@stigok.com
RUN apk --no-cache add certbot
VOLUME /letsencrypt
ENTRYPOINT [ \
        "certbot", "certonly", "--webroot", "-w", "/letsencrypt/public", \
        "--agree-tos", "-m", "stig+letsencrypt@stigok.com", \
        "-n", "--config-dir", "/letsencrypt" \
]
