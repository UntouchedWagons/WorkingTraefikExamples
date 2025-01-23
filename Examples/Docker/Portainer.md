# Portainer

Portainer is a container management app running in Docker

## Sample

```yaml
services:
  portainer:
    image: portainer/portainer-ce:latest
    container_name: portainer
    restart: unless-stopped
    security_opt:
      - no-new-privileges:true
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - portainer:/data
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.portainer.rule=Host(`portainer.docker.YOUR_DOMAIN_NAME.COM`)"
      - "traefik.http.routers.portainer.entrypoints=websecure"
      - "traefik.http.routers.portainer.tls=true"
      - "traefik.http.services.portainer.loadbalancer.server.port=9000"

volumes:
  portainer:
```

## Pressing "Go"

```
# docker compose up -d
```

Then go to `portainer.docker.YOUR_DOMAIN_NAME.COM` in your web browser, set up a new account and add your end points.
