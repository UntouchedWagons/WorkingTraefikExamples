# Jellyfin

Jellyfin is your very own Netflix at home!

## Sample

```yaml
services:
  jellyfin:
    image: jellyfin/jellyfin:10
    container_name: jellyfin
    restart: unless-stopped
    user: 1000:1000
    networks:
      - traefik_default
    volumes:
      - jellyfin:/config
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.jellyfin.rule=Host(`jellyfin.docker.YOUR_DOMAIN_NAME.COM`)"
      - "traefik.http.routers.jellyfin.entrypoints=websecure"
      - "traefik.http.routers.jellyfin.tls=true"
      - "traefik.http.services.jellyfin.loadbalancer.server.port=8096"

volumes:
  jellyfin:
```

## Pressing "Go"

```
# docker compose up -d
```
