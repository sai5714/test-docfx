# Description 

This is classic 2-tier application with frontend that renders server-side. 

- Main storage is Postgres Database `10+ Version`
- Python version `3.10`
- Django version `4.0`.   

# Endpoints

Application consists of two major applications.

1. `/admin/` - Super Admin interface that uses default django authorization mechanism 
and provides full access to all database entities. 

> It implies that this endpoint will be accessible only to certain set of IP address. 
> These rules should be defined on upper level, for instance through extra rules in Kubernetes Ingress or inside nginx config file. 

2. `/` - Main application that uses AppDirect oauth identity provider to grant access  

# Configuration

This project uses [dynaconf](https://www.dynaconf.com/) to meet [12 factor app](https://www.12factor.net/)
requirements in part of [configuration](https://www.12factor.net/config)

All base django settings are stored in `settings.yaml` file in repository root 
and should be treated as final. 
Also, any [Django Setting](https://docs.djangoproject.com/en/4.0/topics/settings/) 
can be overridden in dynaconf manner.  

## Override via environment variables

*Example #1:*

    DATABASES:
      default:
        ENGINE: django.db.backends.sqlite3

Can be overridden with 

    export DJANGO_DATABASES__default__ENGINE=django.db.backends.postgresql

Note: that all variables to override must be prefixed with `DJANGO_`

*Example #2:*

To override env via `.env` put file into workdir with content

    DJANGO_DATABASES__default__ENGINE=django.db.backends.postgresql

> See: docker-compose.env

> Also, you can override settings in different manners, 
> check dynaconf [documentation](https://www.dynaconf.com/configuration/) for certain use case.

# Run development environment

Create empy `.env` file in repository root with content: 

    DJANGO_DEBUG=true
    PYTHONUNBUFFERED=1

Build initial images 

    docker-compose build 

Run composition in development mode

    docker-compose up --build 


# Contributing

## Build initial images 

    docker-compose build

## Poetry

This project uses [poetry](https://python-poetry.org/) as package manager. 

### Lock poetry 

User this snippet to lock inside Docker. Before that you wil

    docker run -it -v $(pwd)/:/tmp/workspace --platform=linux/amd64 --entrypoint=poetry -w /tmp/workspace ad-help-center:local lock
