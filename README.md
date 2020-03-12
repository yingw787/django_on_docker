# `django-on-docker`: Docker Compose tutorial for Django / NGINX / PostgreSQL

I'm using Docker `Docker version 19.03.6, build 369ce74a3c`, and Docker Compose
version `docker-compose version 1.23.2, build 1110ad01`, in order to run this repository.

Clone the repository using:

`git clone https://github.com/yingw787/django-on-docker`, and `cd` into the
repository to run the instructions below.

## Development

To stand up an instance:

```bash
docker-compose up -d --build
```

To tear down an instance:

```bash
docker-compose down -v
```

To lift into an instance:

```bash
docker exec -it $CONTAINER sh # using `sh` instead of `bash` because Alpine instances don't have `bash` installed.
```

To review logs:

```bash
docker logs $CONTAINER
```

## Production

To stand up an instance:

```bash
docker-compose -f docker-compose.prod.yml up -d --build
docker-compose -f docker-compose.prod.yml exec web python manage.py migrate --noinput
docker-compose -f docker-compose.prod.yml exec web python manage.py collectstatic --no-input --clear
```

To tear down an instance:

```bash
docker-compose down -v
```
