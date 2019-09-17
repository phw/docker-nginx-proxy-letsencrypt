## Setup on host

Configure network:

    docker network create -d bridge nginx-proxy

Start services:

    docker-compose -f docker-compose.yml up -d
