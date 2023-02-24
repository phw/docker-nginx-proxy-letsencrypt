# docker-nginx-proxy-letsencrypt

**This project was moved to https://git.sr.ht/~phw/docker-nginx-proxy-letsencrypt**

Ready to use docker-compose configuration to set up [nginx-proxy/docker-gen](https://github.com/nginx-proxy/docker-gen) with [nginx-proxy/acme-companion](https://github.com/nginx-proxy/acme-companion).

Essentially this is the acme-companion [three container setup](https://github.com/nginx-proxy/acme-companion/wiki/Advanced-usage) ready to use with docker-compose.

*Please note: I maintain this only for my own use. I have this setup running on multiple servers,and it fits my need. It might or might not fit yours, but feel free to use and change it.*


## Setup on host

Configure network:

    docker network create -d bridge nginx-proxy

Start nginx proxy services:

    docker compose -f docker-compose.yml up -d


## Configure your project to be exposed via nginx proxy

Given you have a service foo which you want to expose via the nginx proxy
configure it to use the nginx-proxy network and set the environment variables
`VIRTUAL_HOST`, `VIRTUAL_PORT`, `VIRTUAL_NETWORK`, `LETSENCRYPT_HOST` and
`LETSENCRYPT_EMAIL`.

A docker-compose.yml file for this could look like this:

```yml
version: "2.1"

services:
  foo:
    image: foo:latest
    networks:
      - proxy-tier
    environment:
      - VIRTUAL_HOST=foo.example.com
      - VIRTUAL_PORT=80
      - VIRTUAL_NETWORK=proxy-tier
      - LETSENCRYPT_HOST=foo.example.com
      - LETSENCRYPT_EMAIL=admin@foo.example.com

networks:
  proxy-tier:
    external:
      name: nginx-proxy
```


## License

Copyright © 2019-2022 Philipp Wolfer <ph.wolfer@gmail.com>

This work is free. You can redistribute it and/or modify it under the
terms of the Do What The Fuck You Want To Public License, Version 2,
as published by Sam Hocevar. See the [COPYING](./COPYING) file for more details.
