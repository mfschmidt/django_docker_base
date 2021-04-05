# Django on Docker

## Overview

This is a template for a solid django app with postgres database and nginx for static files. But it does nothing. It is designed as a starting point to be forked and extended. See requirements.txt for versions.

It is built on the excellent information at [testdriven.io](https://testdriven.io/blog/dockerizing-django-with-postgres-gunicorn-and-nginx/), where I learned more about using docker-compose than in several years of working with simpler containers.

## Installation and usage

First, clone the project and create a necessary database directory.

```bash
git clone https://github.com/mfschmidt/django_docker_base.git
cd django_docker_base
mkdir postgres_data
```

Fill in environment variables with confidential data, not for git. Change SECRET_KEY, SQL_PASSWORD, POSTGRES_PASSWORD for all configurations.

```bash
vim .env.dev .env.dev.db .env.prod .env.prod.db
```

Then start the development servers and initialize the database.

```bash
# To bring all containers up
docker-compose -f docker-compose.dev.yml up -d --build
docker-compose -f docker-compose.dev.yml exec python manage.py makemigrations
docker-compose -f docker-compose.dev.yml exec python manage.py migrate
```

There should now be a development-configured django site running on [http://localhost:8000/](http://localhost:8000/).

## Helpful commands:

```bash
# To bring all containers up (note .prod indicates production rather than dev)
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

