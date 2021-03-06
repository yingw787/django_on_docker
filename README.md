# `django-on-docker`: Docker Compose tutorial for Django / NGINX / PostgreSQL

From this tutorial:
https://testdriven.io/blog/dockerizing-django-with-postgres-gunicorn-and-nginx/#postgres

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

To check PostgreSQL dev instance:

```bash
docker-compose exec db psql --username=hello_django --dbname=hello_django_dev
```

## Production

To stand up an instance:

```bash
docker-compose -f docker-compose.prod.yml up -d --build
docker-compose -f docker-compose.prod.yml exec web python manage.py migrate --noinput
docker-compose -f docker-compose.prod.yml exec web python manage.py collectstatic --no-input --clear

# exec $CONTAINER is how to move into a container and run instructions as that user (described in the Dockerfile).
```

To tear down an instance:

```bash
docker-compose -f docker-compose.prod.yml down -v # -v to remove the asociated Docker volumes
```

## Deploying to Amazon Web Services

After 5 days of searching, I don't think there's a practical way of deploying
databases onto AWS container services. There were a number of approaches I
wanted to take:

-   Anything with AWS RDS. Out of the question, because I want to use a cron job
    scheduler colocated with the database.

-   EC2 plus package archives / build pipeline + shell script. Eventual dream
    goal, but with changing dependencies it may be a bit difficult to manage.

-   ECS + EBS + Fargate. There were some issues with Fargate, I don't remember
    what they were.

-   ECS + EBS + CloudFormation. Seems to be the ideal development environment.

-   EC2 + ECR + shell script to self-manage docker-compose. Seems to be the
    ideal situation for MVP.

This isn't part of the tutorial, this is me learning AWS ECS deployment.

-   Tutorial supports Docker Compose v3.7 file formats, AWS ECS supports Docker
    Compose v3.0 max at this time.

## Notes

Notes for this tutorial from my hourly journal for Bytes by Ying should be
available here: https://bytes.yingw787.com/categories/tinydevcrm/

## TODOs

- Not using a fully-managed database service, should switch to RDS if / when
  possible
- Using `root` for `db` and `nginx` services, should use non-root user like in
  `web`
