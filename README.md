# Django on Docker

## Overview

This is a template for a solid django app with postgres database and nginx for static files. But it does nothing. It is designed as a starting point to be forked and extended. See requirements.txt for versions.

It is built on the excellent information at [testdriven.io](https://testdriven.io/blog/dockerizing-django-with-postgres-gunicorn-and-nginx/), where I learned more about using docker-compose than in several years of working with simpler containers.

## Helpful commands:

```bash
# To bring all containers up
docker-compose -f docker-compose.prod.yml up -d --build
```

```bash
# To monitor processes in containers and check for errors:
docker-compose -f docker-compose.prod.yml logs -f
```

```bash
# To run things on the container, like django database migrations
docker-compose -f docker-compose.prod.yml exec web python manage.py migrate --noinput
```

```bash
# To take all containers down
docker-compose -f docker-compose.prod.yml down -v
```

