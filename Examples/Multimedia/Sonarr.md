# Sonarr

Sonarr is a web app for managing and downloading movies.

## Sample

```yaml
services:
  sonarr:
    image: ghcr.io/onedr0p/sonarr:5
    container_name: sonarr
    restart: unless-stopped
    user: 1000:1000
    networks:
      - traefik_default
    volumes:
      - sonarr:/config
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.sonarr.rule=Host(`sonarr.docker.YOUR_DOMAIN_NAME.COM`)"
      - "traefik.http.routers.sonarr.entrypoints=websecure"
      - "traefik.http.routers.sonarr.tls=true"
      - "traefik.http.services.sonarr.loadbalancer.server.port=8989"

volumes:
  sonarr
```

## Pressing "Go"

```
# docker compose up -d
```
