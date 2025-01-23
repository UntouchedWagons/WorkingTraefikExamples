# Prowlarr

Prowlarr is a web app for synchronizing torrent trackers and Usenet indexers in Lidarr, Sonarr and Radarr.

## Sample

```yaml
services:
  prowlarr:
    image: ghcr.io/onedr0p/prowlarr:1
    container_name: prowlarr
    restart: unless-stopped
    user: 1000:1000
    networks:
      - traefik_default
    volumes:
      - prowlarr:/config
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.prowlarr.rule=Host(`prowlarr.docker.YOUR_DOMAIN_NAME.COM`)"
      - "traefik.http.routers.prowlarr.entrypoints=websecure"
      - "traefik.http.routers.prowlarr.tls=true"
      - "traefik.http.services.prowlarr.loadbalancer.server.port=9696"

volumes:
  prowlarr
```

## Pressing "Go"

```
# docker compose up -d
```
