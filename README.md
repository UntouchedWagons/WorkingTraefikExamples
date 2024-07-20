# Working Traefik Examples

A selection of complete, working, traefik examples in docker.

## Background

In my attempts to set up Traefik in Docker, I found the official documentation unhelpful due to a severe lack of functional examples. So I made this repository to document what I've found to work for a functional Traeifk setup in Docker

## Requirements and understandings

These examples assume you're already familiar with Let's Encrypt, Docker and Docker Compose. I also assume you already own a domain name that is registered with a registrar that Let's Encrypt supports and you know how to get a token for editing DNS records. In the examples I provide, I will be using Cloudflare.

## Getting started

This is my working Traefik setup. This has been tested to work with Traefik 3.1.0

```yaml
services:
  traefik:
    image: public.ecr.aws/docker/library/traefik:latest
    container_name: "traefik"
    restart: unless-stopped
    command:
      #- --log.level=DEBUG
      - --api.insecure=true
      - --api.dashboard=true
      - --providers.docker=true
      - --providers.docker.exposedbydefault=false
      - --entrypoints.web.address=:80
      - --entrypoints.websecure.address=:443
      - --entrypoints.web.http.redirections.entryPoint.to=websecure
      - --entrypoints.web.http.redirections.entryPoint.scheme=https
      - --entrypoints.web.http.redirections.entrypoint.permanent=true
      - --entrypoints.websecure.http.tls.domains[0].main=docker.YOUR_DOMAIN_NAME.COM
      - --entrypoints.websecure.http.tls.domains[0].sans=*.docker.YOUR_DOMAIN_NAME.COM
      - --entrypoints.websecure.http.tls.certresolver=myresolver
      - --certificatesresolvers.myresolver.acme.dnschallenge=true
      - --certificatesresolvers.myresolver.acme.dnschallenge.provider=cloudflare
      - --certificatesresolvers.myresolver.acme.dnschallenge.resolvers=9.9.9.9:53
      - --certificatesresolvers.myresolver.acme.email=YOUR_EMAIL_ADDRESS@GMAIL.COM
      - --certificatesresolvers.myresolver.acme.storage=/etc/traefik/acme.json
      - --serverstransport.insecureskipverify=true
    ports:
      - 80:80
      - 443:443
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.dashboard.rule=Host(`dashboard.docker.YOUR_DOMAIN_NAME.COM`)"
      - "traefik.http.routers.api.service=api@internal"
      - "traefik.http.services.dashboard.loadbalancer.server.port=8080"
      - "traefik.http.routers.dashboard.tls=true"
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Toronto
      - CF_DNS_API_TOKEN=YOUR_CLOUDFLARE_DNS_TOKEN
    volumes:
      - ./appdata/traefik:/etc/traefik
      - /var/run/docker.sock:/var/run/docker.sock:ro
```
  
  Some things to note in this example: 
  
   * Enter the domain name you own in the three places it says `YOUR_DOMAIN_NAME.COM` 
   * Enter your email address where it says `YOUR_EMAIL_ADDRESS@GMAIL.COM` 
   * Enter the token you got from Cloudflare where it says `YOUR_CLOUDFLARE_DNS_TOKEN`
   * Edit the timezone to match your area (The author is unsure what this actually affects)
   * Edit the PUID and PGID if needed. Docker containers should not run as root unless they absolutely have to, Traefik is not such a container. 1000:1000 is the UID:GID of the user I use to log into my docker VM
   
## Pressing "Go"

```
# mkdir -p ./appdata/traefik
# docker compose up -d
```

Give Traefik a minute to get a certificate and set up routes. Then go to `https://dashboard.docker.YOUR_DOMAIN_NAME.COM` in your web browser and you should get the Traefik dashboard using the SSL certificate provided by Let's Encrypt!

## Container examples

 * [Portainer](Portainer.md)
 * [Omada](Omada.md)
 * [Unifi](Unifi.md)
