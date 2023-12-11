# Nginx-Proxy

This project is a deployment of [nginx-proxy](https://github.com/nginx-proxy/nginx-proxy) and [acme-companion](https://github.com/nginx-proxy/acme-companion) for a reverse proxy nginx with automated SSL certificates creation/renewal

## Services

- `nginx-proxy`: Main nginx-proxy service that generates reverse-proxy configs. It uses the following volumes:
    - `/var/run/docker.sock:/tmp/docker.sock:ro`
    - `./certs:/etc/nginx/certs`
    - `./html:/usr/share/nginx/html`
    - `./vhost:/etc/nginx/vhost.d`

- `nginx-proxy-acme`: Manages ACME (Automated Certificate Management Environment) for nginx-proxy. It uses the following volumes:
    - `/var/run/docker.sock:/var/run/docker.sock:ro`
    - `./acme:/etc/acme.sh`
    - `./certs:/etc/nginx/certs`
    - `./html:/usr/share/nginx/html`
    - `./vhost:/etc/nginx/vhost.d`

## Environment Variables

- `DEFAULT_EMAIL`: This should be set to your email address.
- `NGINX_PROXY_CONTAINER`: This should be set to the name of the Nginx proxy container (`nginx-proxy` for our example).

## Running the Services

To start the services, run `docker-compose up -d`.

## Usage
Once both **nginx-proxy** and **nginx-proxy-acme** containers are up and running, start any container you want proxied with environment variables `VIRTUAL_HOST` (control proxying) and `LETSENCRYPT_HOST` (control certificate creation and SSL enabling) both set to the domain(s) your proxied container is going to use.

```shell
$ docker run --detach \
    --name your-proxied-app \
    --env "VIRTUAL_HOST=subdomain.yourdomain.tld" \
    --env "VIRTUAL_PORT=80" \
    --env "LETSENCRYPT_HOST=subdomain.yourdomain.tld" \
    --env "LETSENCRYPT_EMAIL=mail@yourdomain.tld" \
    nginx
```

## Stopping the Services

To stop the services, run `docker-compose down`.
