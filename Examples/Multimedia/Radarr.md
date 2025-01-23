# Radarr

Radarr is a web app for managing and downloading movies.

## Sample

```yaml
services:
  radarr:
    image: ghcr.io/onedr0p/radarr:5
    container_name: radarr
    restart: unless-stopped
    user: 1000:1000
    networks:
      - traefik_default
    volumes:
      - radarr:/config
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.radarr.rule=Host(`radarr.docker.YOUR_DOMAIN_NAME.COM`)"
      - "traefik.http.routers.radarr.entrypoints=websecure"
      - "traefik.http.routers.radarr.tls=true"
      - "traefik.http.services.radarr.loadbalancer.server.port=7878"

volumes:
  radarr
```

## Pressing "Go"

```
# docker compose up -d
```
