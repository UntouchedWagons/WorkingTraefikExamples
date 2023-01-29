# Omada Controller

The Omada Controller is the software needed for managing some of TP-Link's networking equipment

## Sample

    version: '3'

    services:
      omada:
        container_name: omada
        restart: unless-stopped
        image: "mbentley/omada-controller:latest"
        ports:
        - 8088:8088
        - 8043:8043
        - 8843:8843
        - 29810:29810/udp
        - 29811:29811
        - 29812:29812
        - 29813:29813
        - 29814:29814
        environment:
        - PGID=1000
        - PUID=1000
        - MANAGE_HTTP_PORT=8088
        - MANAGE_HTTPS_PORT=443
        - PORTAL_HTTP_PORT=8088
        - PORTAL_HTTPS_PORT=8843
        - SHOW_SERVER_LOGS=true
        - SHOW_MONGODB_LOGS=false
        - SSL_CERT_NAME=tls.crt
        - SSL_KEY_NAME=tls.key
        - TZ=America/Toronto
        volumes:
        - ./appdata/omada:/opt/tplink/EAPController/data
        - omada-logs:/opt/tplink/EAPController/logs
        labels:
        - "traefik.enable=true"
        - "traefik.http.routers.omada.rule=Host(`omada.docker.YOUR_DOMAIN_NAME.COM`)"
        - "traefik.http.routers.omada.entrypoints=websecure"
        - "traefik.http.routers.omada.tls=true"
        - "traefik.http.services.omada.loadbalancer.server.port=443"
        - "traefik.http.services.omada.loadbalancer.server.scheme=https"
        - "traefik.http.routers.omada.middlewares=omada-middlewares"
        - "traefik.http.middlewares.omada-middlewares.chain.middlewares=omada-redirect,omada-headers"
        - "traefik.http.middlewares.omada-headers.headers.customrequestheaders.Host=omada.docker.YOUR_DOMAIN_NAME.COM:443"
        - "traefik.http.middlewares.omada-headers.headers.customresponseheaders.Host=omada.docker.YOUR_DOMAIN_NAME.COM"
        - "traefik.http.middlewares.omada-redirect.redirectregex.regex=^https:\\/\\/([^\\/]+)\\/?$$"
        - "traefik.http.middlewares.omada-redirect.redirectregex.replacement=https://$$1/login"

    volumes:
      omada-logs:

Some things to note in this example:

 * The MANAGE_HTTPS_PORT must be set to 443.

## Pressing "Go"

```
# docker compose up -d
```
