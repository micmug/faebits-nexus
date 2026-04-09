# FaeBits Server Infrastruktur (Digital Twin)

Dieses Dokument ist ein Echtzeit-Abbild der laufenden Server-Infrastruktur zum Zeitpunkt der Repository-Schöpfung.
Nutze dieses Wissen, um deinen Code exakt auf diese Umgebung abzustimmen.

## Laufende Docker Services (Auszug)
```text
- nanobot-gateway (ai-stack-nanobot-gateway)
- webhook-gateway (ai-stack-webhook-gateway)
- litellm (ghcr.io/berriai/litellm:main-latest)
- litellm-db (postgres:15-alpine)
- ai-dockerproxy (tecnativa/docker-socket-proxy)
- qdrant (qdrant/qdrant:v1.17.0)
- dozzle (amir20/dozzle:latest)
- traefik (traefik:v3)
- gitea-runner (gitea/act_runner:0.3.0)
- faebits-3d (nginx:1.27-alpine)
- faebits-pixel (nginx:1.27-alpine)
- faebits-classic (nginx:1.27-alpine)
- faebits-staging (nginx:1.27-alpine)
- gitea (gitea/gitea:latest)
- postgres (postgres:15-alpine)
```

## Bekannte Traefik Routen & Docks
```text
- 3d.faebits.com -> faebits-3d
- faebits.com -> faebits-classic
- staging.faebits.com -> faebits-staging
- git.faebits.com -> gitea
- pixel.faebits.com -> faebits-pixel
```
