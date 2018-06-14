# Hasura GraphQL Engine using docker-compose

## Prerequisites

- [Docker](https://docs.docker.com/install/)
- [Docker Compose](https://docs.docker.com/compose/install/#install-compose)

## Deploy

- Create a docker-compose.yaml file and copy the below configuration into the file.
  ```yaml
  version: '3.6'
  services:
    postgres:
      image: postgres
      environment:
      - "POSTGRES_PASSWORD:mysecretpassword"
      volumes:
        - db_data:/var/lib/postgresql/data
    raven:
      image: hasuranightly/raven:94a0141
      ports:
      - "9000:8080"
      links:
      - "postgres:postgres"
      command: >
        /bin/sh -c "
        sleep 5;
        raven --database-url postgres://postgres:mysecretpassword@postgres:5432/postgres serve;
        "
  volumes:
    db_data:

  ```

- Run `docker-compose up -d` to start the Hasura GraphQL Engine.

## Configure

(Assuming you have already executed `hasura init`)

- Edit `config.yaml` and add `endpoint`
  ```yaml
  endpoint: http://localhost:9000
  ```

## Console

Open the console and start exploring APIs / manage tables/views:
```bash
hasura console
```
