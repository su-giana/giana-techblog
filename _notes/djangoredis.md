---
title: Django with Redis
---

## Docker setting

```dockerfile
# Use Ubuntu 18.04 as image base
FROM ubuntu:18.04

# Ubuntu setting
RUN apt-get update
RUN apt-get install -y software-properties-common
RUN apt-get install -y git
RUN apt-get install -y apt-utils
RUN apt-get install -y vim
RUN add-apt-repository ppa:deadsnakes/ppa

# Install python
RUN apt-get update
RUN apt install -y python3.5
RUN apt install -y python3.6
RUN apt install -y python3-pip
RUN apt install -y python3.6-dev libpq-dev

# python default version 3.5 -> 3.6
RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.5 1
RUN update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.6 2
RUN update-alternatives --config python3

# net-tools, dnsutils
RUN apt-get install -y net-tools
RUN apt-get install -y dnsutils
RUN apt-get update

# remove python output buffer
ENV PYTHONUNBUFFERED 0

# set working directory
WORKDIR /home/flooDetect

# install requirements.txt
COPY requirements.txt /home/flooDetect
RUN pip3 install --upgrade pip
RUN pip3 install -r requirements.txt

# script for postgres
ADD https://raw.githubsercontent.com/vishnubob/wait-for-it/master/wait-for-it.sh /
```

run with "docker build -t django ."

```shell
python3 -m venv .venv # 다른 가상환경 사용해도 무방
source .venv/bin/activate
pip install --upgrade pip # 필요하다면
pip install django
pip install psycopg2-binary
pip install djangorestframework 
pip install django-redis # redis cache 
pip install django-rq # redis message que (broker)
django-admin startproject {project name}
cd {project name}
python manage.py startapp {application name}
```

docker-compose file
```docker
version: "3"

volumes:
  postgresql_db_dev: {}

# container which we will run
services:
  #Database container
  postgres:
    container_name: postgres_services
    image: postgres:9.5
    volumes:
      - ./data/postgres/:/var/lib/postgresql/data
    environment:
      - POSTGRES_DB = gigidb
      - POSTGRES_USER = "gigi"
      - POSTGRES_PASSWORD = "gigigigi"
      - POSTGRES_INITDB_ARGS=--encoding=UTF-8

  # Django container
  django:
    container_name : django_service
    build:
      context: .
      dockerfile: ./Dockerfile
    environment:
      - POSTGRES_NAME = gigidb
      - POSTGRES_USER = "gigi"
      - POSTGRES_PASSWORD = "gigigigi"
    ports:
      - "8000:8000"
    volumes:
      - ./:/home/flooDetect
    command: python3 manage.py runserver
    depends_on:
      - redis

  redis:
    container_name: redis_service
    image: redis
    ports:
      - "6379:6379"
    
```

Run "docker-compose up -d"