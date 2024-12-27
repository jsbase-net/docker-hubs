# Postgres Images for developers

## Build

```sh
docker build -t postgres-common .
docker run -d -p 5432:5432 \
    --name postgres-container \
    -e POSTGRES_USER=jsbase \
    -e POSTGRES_PASSWORD=123456 \
    postgres-common
docker exec -it postgres-container psql -U jsbase -c "SELECT extname FROM pg_extension;"
```

## Push

```sh
docker login
docker tag postgres-common sonnm/postgres-common:16.6
docker push sonnm/postgres-common:16.6
# latest version
docker tag postgres-common sonnm/postgres-common
docker push sonnm/postgres-common
```

## Usage

You can start your container with this command

```sh
docker run -d -p 5432:5432 \
    --name postgres-container \
    -e POSTGRES_USER=jsbase \
    -e POSTGRES_PASSWORD=jsbase \
    sonnm/postgres-common:16.6
```

You can also list existing pg_extension

```sh
docker exec -it postgres-container psql -U jsbase -c "SELECT extname FROM pg_extension;"
```

Sample with docker-compose

```sh
services:
  postgres:
    container_name: jsbase_postgres_local
    image: sonnm/postgres-common
    shm_size: 1g
    restart: unless-stopped
    ports:
      - "5432:5432"
    environment:
      POSTGRES_USER: jsbase
      POSTGRES_PASSWORD: 123456
    volumes:
      - jsbase_postgres_local:/var/lib/postgresql/data
    networks:
      - jsbase
  pgadmin:
    container_name: jsbase_pgadmin_local
    image: dpage/pgadmin4:8.0
    shm_size: 1g
    restart: unless-stopped
    depends_on:
      - postgres
    ports:
      - "5050:80"
    environment:
      PGADMIN_DEFAULT_EMAIL: admin@postgres.com
      PGADMIN_DEFAULT_PASSWORD: 123456
      PGADMIN_CONFIG_SERVER_MODE: "False"
    volumes:
      - jsbase_pgadmin_local:/var/lib/pgadmin
    networks:
      - jsbase
networks:
  jsbase:
volumes:
  jsbase_postgres_local:
  jsbase_pgadmin_local:
```
