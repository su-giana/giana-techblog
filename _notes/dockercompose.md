---
title: "docker-compose"
---

## 1. What is Docker compose
> Docker compose is a tool provided by Docker that allows you to define and manage multi-container Docker applications.
- It uses a YAML file to specify the configuration, dependencies, and relationships between the containers in your application stack
- docker-compose up : start and initialize the containers defined in the Docker file
- docker-compose down : stop and remove the containers

#### Difference between Dockerfile and docker-compose
> Dockerfile is used to build a single Docker image, while Docker Compose is used to define and manage multiple Docker containers as an application stack
- Dockerfile focuses on the image creation process, while Docker Compose focuses on the orchestration and coordination of containers

#### Install Docker Compose
```
sudo apt install -y python3 python3-pip
sudo pip3 install docker-compose
```

<hr>

## 2. How to write Docker Compose file
- The name of file should be "**docker-compose.yml**"
- Indextation, colons, and spaces are essential for proper interpretation.
=> spaces should be fixed, space after colon
```yml
version: "3"
services:
  mysql000ex11:
    image: mysql:5.7
    networks:
      - wordpress000net1
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: {}
      MYSQL_DATABASE: wordpress000db
      MYSQL_USER: {user}
      MYSQL_PASSWORD: {password}
  wordpress000ex12:
    depends_on:
      - mysql1000ex11
    image: wordpress
    networks:
      - wordpress000net1
    volumes:
      - wordpress000vol12:/var/www/html
    ports:
      - 8085:80
    restart: always
    environment:
      WORDPRESS_DB_HOST: mysql000ex11
      WORDPRESS_DB_NAME: wordpress000db
      WORDPRESS_DB_USER: wordpress000kun
      WORDPRESS_DB_PASSWORD: {password}
networks:
  wordpress000net1:
volumes:
  mysql000vol11:
  wordpress000vol12:
```

### docker-compose execution
```
docker-compose -f {file_path} up {options}
docker-compose -f {file_path} down {options}
```
