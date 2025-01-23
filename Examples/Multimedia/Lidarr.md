# Lidarr

Lidarr is a web app for managing and downloading music.

## Sample

```yaml
services:
  lidarr:
    image: ghcr.io/onedr0p/lidarr:2
    container_name: lidarr
    restart: unless-stopped
    user: 1000:1000
    networks:
      - traefik_default
    volumes:
      - lidarr:/config
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.lidarr.rule=Host(`lidarr.docker.YOUR_DOMAIN_NAME.COM`)"
      - "traefik.http.routers.lidarr.entrypoints=websecure"
      - "traefik.http.routers.lidarr.tls=true"
      - "traefik.http.services.lidarr.loadbalancer.server.port=8686"

volumes:
  lidarr
```

## Pressing "Go"

```
# docker compose up -d
```
