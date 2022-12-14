
# Traefik v2, Lets-Encrypt, Docker-Compose
This repository contains a traefik docker-compose configuration.

## Running the project 

To run the project the following steps must be followed: 

- Change .env file according to your settings ; for the TRAEFIK and PORTAINER dashboards you need to create a subdomain on top of your main domain, eg "traefik.mydomain.org"

- Run the services:

```
docker-compose up -d
```

Important Notes: 

To generate a new passoword traefik dashboard the following line in docker-compose must be updated: 

``
"traefik.http.middlewares.traefik-auth.basicauth.users=admin:$$2y$$05$$GGeLEnClIsWmz8QTr5LjfuhjD.RN4Ym3f/t1Pfjl2jQGI4Fx7UOTm"
``

A new password can be generated using the following command: 

``` 
echo $(htpasswd -nB admin) | sed -e s/\\$/\\$\\$/g
```

Or use the shell script:
```
generate_traefik_basic_auth.sh
``` 

## Add other services

To make other services visible labels have to be added like below: 

```
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.portainer.rule=Host(`portainer.domain.com`)"
      - "traefik.http.routers.portainer.entrypoints=websecure"
      - "traefik.http.routers.portainer.tls.certresolver=myhttpchallenge"
      - "traefik.http.routers.portainer.tls=true"
      - "traefik.http.services.portainer.loadBalancer.server.port=9000"
    # UNCOMMENT THIS ONLY IF YOU THE SERVICE IS EXPOSED WITH A CONTEXT
    # - "traefik.http.routers.movies.rule=PathPrefix(`/api`)"
  
```
<b>Important</b>: 
For ``..routers.portainer.rule..``</b> lines <b>portainer</b> must be changed for every service. 
(ex, ``..routers.geoserver.rule..``, ``..routers.api.rule..`` etc). 
It can be the same as docker-compose services name or different. 

The services can be added in the same docker-compose file or different docker-compose files as long as the services are within the same network. 

## Credits

Traefik configuration from https://github.com/jnsgruk/infra.