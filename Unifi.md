# Unifi Controller

The Unifi Controller is the software needed for managing Ubiquiti's networking equipment

## Sample

    version: '3'

    services:
      unifi:
        image: linuxserver/unifi-controller:7.3.76
        container_name: unifi
        restart: unless-stopped
        environment:
        - PUID=1000 # for UserID
        - PGID=1000 # for GroupID
        - MEM_LIMIT=1024 # Optionally change the Java memory limit (-Xmx) (default is 1024M).
        volumes:
        - ./appdata/unifi:/config # All Unifi data stored here
        ports:
        - 10001:10001/udp # Required for AP discovery
        - 8443:8443/tcp # Unifi web admin port
        - 8080:8080/tcp # Required for device communication
        - 6789:6789/tcp # For mobile throughput test
        - 5514:5514/tcp # Remote syslog port
        - 3478:3478/udp # Unifi STUN port
        - 1900:1900/udp # Required for Make controller discoverable on L2 network option
        networks:
        - default
        - traefik_default
        labels:
        - "traefik.enable=true"
        - "traefik.http.routers.unifi.rule=Host(`unifi.docker.YOUR_DOMAIN_NAME.COM`)"
        - "traefik.http.routers.unifi.entrypoints=websecure"
        - "traefik.http.routers.unifi.tls=true"
        - "traefik.http.services.unifi.loadbalancer.server.port=8443"
        - "traefik.http.services.unifi.loadbalancer.server.scheme=https"

## Pressing "Go"

```
# docker compose up -d
```
