# ct-proxy

This small repository contains the reverse proxy configuration used to route
requests to the various micro‑services that make up the auto.colagemtravel.com
platform. It is intentionally separate from any of the service repositories so
the proxy settings can evolve independently.

## Components

* `docker-compose.yml` – starts a Caddy container listening on ports 80/443.
* `Caddyfile` – defines host/path routes and downstream service addresses.

## Usage

1. Make sure every service you want to expose joins the `proxy-net` network
   (either by declaring it as an external network in its own compose or by
   using `docker network connect` after start).
2. Add or modify route blocks in `Caddyfile` pointing to the appropriate
   container name and internal port.
3. Run `docker-compose up -d` from this directory to start the proxy.

For local development you can also build and run each micro‑service alongside
this proxy; only the proxy binds port 80/443 on your host.

## Examples

```yaml
# other service's compose
services:
  webhook:
    build: ../ct-webhook
    networks:
      - proxy-net

networks:
  proxy-net:
    external: true
```

Then the proxy can forward `/webhook` requests to `webhook:3000` as defined in
this repo's `Caddyfile`.
