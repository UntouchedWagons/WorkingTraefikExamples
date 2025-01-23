# Unifi Controller

The Unifi Controller is the software needed for managing Ubiquiti's networking equipment

## Sample

```yaml
services:
  mongo:
    image: docker.io/mongo:7.0
    container_name: mongo
    environment:
      - MONGO_INITDB_ROOT_USERNAME=root
      - MONGO_INITDB_ROOT_PASSWORD=CorrectHorseBatteryStaple
      - MONGO_USER=unifi
      - MONGO_PASS=CorrectHorseBatteryStaple
      - MONGO_DBNAME=unifi
      - MONGO_AUTHSOURCE=admin
    volumes:
      - mongo:/data/db
      - ./init-mongo.sh:/docker-entrypoint-initdb.d/init-mongo.sh:ro
    restart: unless-stopped

  unifi:
    image: lscr.io/linuxserver/unifi-network-application:latest
    container_name: unifi
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Toronto
      - MONGO_USER=unifi
      - MONGO_PASS=CorrectHorseBatteryStaple
      - MONGO_HOST=mongo
      - MONGO_PORT=27017
      - MONGO_DBNAME=unifi
      - MONGO_AUTHSOURCE=admin
      - MEM_LIMIT=1024 #optional
      - MEM_STARTUP=1024 #optional
      - MONGO_TLS= #optional
    volumes:
      - unifi:/config
    depends_on:
      mongo:
        condition: service_started
    ports:
      - 8443:8443
      - 3478:3478/udp
      - 10001:10001/udp
      - 8080:8080
      - 1900:1900/udp #optional
      - 8843:8843 #optional
      - 8880:8880 #optional
      - 6789:6789 #optional
      - 5514:5514/udp #optional
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.unifi.rule=Host(`unifi.docker.YOUR_DOMAIN_NAME.COM`)"
      - "traefik.http.routers.unifi.entrypoints=websecure"
      - "traefik.http.routers.unifi.tls=true"
      - "traefik.http.services.unifi.loadbalancer.server.scheme=https"
      - "traefik.http.services.unifi.loadbalancer.server.port=8443"
    restart: unless-stopped

volumes:
  mongo:
  unifi:
```

## Pressing "Go"

```
# docker compose up -d
```
