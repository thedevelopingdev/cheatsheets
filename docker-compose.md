# Docker Compose cheatsheet

## Version 3

- references: https://docs.docker.com/compose/compose-file/

```yaml
version: "3"

services:
  <your service name>:
    image: repo/image:tag
    ports:
      - "<host port>:<container port>"
  web:
    image: node:12
    ports:
      - "3000:3000"

networks:
  <network-name>: {}
  my-network-1: {}

# find where volumes are stored with `docker volume inspect <volume>`
# default is `/var/lib/docker/volumes`
volumes:
  <volume-name>: {}
  my-volume-1: {}
  my-volume-2: {}
```
