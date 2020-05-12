# Install Kong

## Prepare install
```
$ docker pull postgres:9.6
$ docker pull kong
$ docker pull pantsel/konga
$ docker network create kong-net
```

## Start Database

```
$ docker run -d --name kong-database \
               --network=kong-net \
               -p 5432:5432 \
               -e "POSTGRES_USER=kong" \
               -e "POSTGRES_DB=kong" \
               -e "POSTGRES_PASSWORD=kong" \
               postgres:9.6
```

Run the migrations with an ephemeral Kong container:
```
$ docker run --rm \
     --network=kong-net \
     -e "KONG_DATABASE=postgres" \
     -e "KONG_PG_HOST=kong-database" \
     -e "KONG_PG_PASSWORD=kong" \
     kong:latest kong migrations bootstrap
```

## Start Kong
```
$ docker run -d --name kong \
     --network=kong-net \
     -e "KONG_DATABASE=postgres" \
     -e "KONG_PG_HOST=kong-database" \
     -e "KONG_PG_PASSWORD=kong" \
     -e "KONG_PROXY_ACCESS_LOG=/dev/stdout" \
     -e "KONG_ADMIN_ACCESS_LOG=/dev/stdout" \
     -e "KONG_PROXY_ERROR_LOG=/dev/stderr" \
     -e "KONG_ADMIN_ERROR_LOG=/dev/stderr" \
     -e "KONG_ADMIN_LISTEN=0.0.0.0:8001, 0.0.0.0:8444 ssl" \
     -p 8000:8000 \
     -p 8443:8443 \
     -p 127.0.0.1:8001:8001 \
     -p 127.0.0.1:8444:8444 \
     kong:latest
```
Check Kong is running:
```
$ curl -i http://localhost:8001/
```

## Start Konga
```
docker run -d --rm \
     --network=kong-net \
     -e "TOKEN_SECRET=nino" \
     -e "NODE_ENV=production" \
     -p 8080:1337 \
     pantsel/konga
```

Open Konga in web
```
http://localhost:8080
```