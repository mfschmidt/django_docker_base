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

Fill in environment variables with confidential data, not for git. Change the SECRET_KEY, SQL_PASSWORD, POSTGRES_PASSWORD values for all configurations. The following instructions will use the same SECRET_KEY for both development and production. They will also use the same password for both development and production databases. You can use different values for each by changing values in .dev and .prod files separately. Obviously, use your own secret values in the following two commands.

```bash
sed -i 's/SECRET_KEY_VALUE/a_secret_key_generated_without_forward_slashes_or_double_quotes/g' .env.*
sed -i 's/DB_PASSWORD_VALUE/my_own_password/g' .env.*
```

Then start the development servers and initialize the database.

```bash
# To bring all containers up
docker-compose -f docker-compose.dev.yml up -d --build
docker-compose -f docker-compose.dev.yml exec python manage.py makemigrations
docker-compose -f docker-compose.dev.yml exec python manage.py migrate

# To take it back down
docker-compose -f docker-compose.dev.yml down -v
```

There should now be a development-configured django site running on [http://localhost:8000/](http://localhost:8000/). Run the same three commands, replacing .dev. with .prod. to start the production site with the same code and its own database. All configuration can be done by modifying .env.*'s, Dockerfile's and docker-compose.yml's.

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

