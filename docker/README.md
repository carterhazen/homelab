# Docker

This is where my homelab started, and until I am fully comfortable with Kubernetes the services my users and I use everyday will continue to run here.

<br>

# Architecture

Everything runs in a single docker environment via Docker Compose.  Containers are segregated via docker networking as shown below.

![docker networks](../readme_images/dockernetworks.jpg)

<br>

# Containers

Each service has its own folder where the associated compose file lives.

## Caddy

This is the external-facing reverse proxy.  It is running a custom caddy docker image that implements various features that are helpful for handling requests from the open internet.

### Networking

The docker network mode on this container is IPvlan.  This allows the container to receive a dedicated IP on my local network separate from the physical docker host.  It's not absolutely necessary but it does make creating and tracking DNS entries easier as public requests come in and get routed to only the exposed Caddy container IP.  Looking at my DNS entries I can easily tell which ones are routing to the exposed Caddy container, and incoming requests from the public internet are kept separate from my docker host's primary IP.

This IPvlan proxy network is only connected to services I'm comfortable exposing to the internet, and each has a corresponding route definition within the Caddyfile.

Routing within docker networking is also nice because you can define endpoints via container name.  Docker networking will check its local DNS first and route to the appropriate container just based off name & port like `jellyfin:8096`, which keeps the DNS entries on your firewall from getting cluttered.

## Caddyint

## Crowdsec

## Jellyfin

## Komodo

## Minecraft