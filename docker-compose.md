# Docker Compose cheatsheet

## Version 3

- references: https://docs.docker.com/compose/compose-file/

```yaml
version:
services:
  <your service name>:
    image: repo/image:tag
    ports:
      - "<host port>:<container port>"


networks:

# find where volumes are stored with `docker volume inspect <volume>`
# default is `/var/lib/docker/volumes`
volumes:
```
