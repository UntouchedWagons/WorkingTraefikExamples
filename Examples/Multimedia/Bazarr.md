# Bazarr

Bazarr is a web app for managing and downloading subtitles for movies and tv shows.

## Sample

```yaml
services:
  bazarr:
    image: ghcr.io/onedr0p/bazarr:1
    container_name: bazarr
    restart: unless-stopped
    user: 1000:1000
    networks:
      - traefik_default
    volumes:
      - bazarr:/config
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.bazarr.rule=Host(`bazarr.docker.YOUR_DOMAIN_NAME.COM`)"
      - "traefik.http.routers.bazarr.entrypoints=websecure"
      - "traefik.http.routers.bazarr.tls=true"
      - "traefik.http.services.bazarr.loadbalancer.server.port=6767"

volumes:
  bazarr:
```

## Pressing "Go"

```
# docker compose up -d
```
