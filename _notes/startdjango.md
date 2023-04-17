---
title: Start Django in Macbook M1 Pro (Gunicorn + Nginx + Django)
---

## 1. Virutal Environment
Make a virtual environment
```shell
python3 -m venv {virtual environment name}
```

Activate the virtual environment
```shell
source {virtual environment name}/bin/activate
```

## 2. Gunicorn
### Why we use Gunicorn
> Django's runserver is not appropriate to real service
Due to limit of performace and security, most of companies are using **was + wsgi combination (Nginx and Gunicorn) in real use
In Django.core.servers.basehttp file, there is a warning statement that "Don't use it for production use!"

### Installation
```shell
$ pip install gunicorn
```

### Execution
```shell
$ gunicorn --preload --bind 0:8000 {application name}.wsgi:application
```
--preload : specify detailed information about booting error
> This statement is similar with "runserver" statement of dafult django system

### Characteristic of Nginx
- Nginx use Event-driven Architecture
=> fast memory generation speed and process much more clients with smaller thread
- After change configuration, Nginx sends reload signal without restart of server daemon
- Non blocking event driven network communication

#### Installation and Execution
```
$ brew install nginx
$ brew services start nginx
$ brew services stop nginx
```

#### Configuration
configuration file : /usr/local/etc/nginx/nginx.conf or **/opt/homebrew/etc/nginx/**
directory which contains nginx file : /usr/local/Cellar/nginx/1.19.9