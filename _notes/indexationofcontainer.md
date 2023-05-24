---
title: "Indexation of Container"
---

## Practice with Wordpress
Wordpress is the best instance to practice container network because it requires some programs such as apache, database and php runtime. We will implement worldpress website using wordpress container and MySQL container.
First of all, we need virtual network to connect two containers. The command is 

```
docker network create {network name}
docker network rm {network name}
docker network connect, disconnect, inspect, ls, prune .. 
```

### Practice.

```
docker network create wordpress000net1

docker run --name mysql1000ex11 -dit --net=wordpress000net1 -e MYSQL_ROOT_PASSWORD=myrootpass -e MYSQL_DATABASE=wordpress000db -e MYSQL_USER=wordpress000kun -e MYSQL_PASSWORD=wkunpass mysql --character-set-server=utf8m64 --collation-server=utfmb4_unicode_ci --default-authentication-plugin=mysql_navtive_password

docker run --name wordpress000ex12 -dit --network=wordpress000net1 -p 8085:80 -e WORDPRESS_DB_HOST=mysql000ex11 -e WORDPRESS_DB_NAME=wordpress000db -e WORDPRESS_DB_USER=wordpress000kun WORDPRESS+DB+PASSWORD=wkunpass wordpress
```

### Redmine + MySQL practice

```docker
docker network create redmine000net12

docker run --name mysql000ex13 -dit --net=remine000net2 -e MYSQL_ROOT_PASSWORD=myrootpass -e MYSQL_DATABASE=redmine000db -e MYSQL_USER=redmine000kun -e MYSQL_PASSWORD=rkunpass mysql --character-set-server=utf8m64 --collation-server=utf8mb4_unicode_ci --default-authentication-plugin=mysql_native_password

docker run -dit redmine000ex14 --network remine000net2 -p 8086:3000 -e REDMINE_DB_MYSQL=mysql000ex13 REMINE_DBDATABASE=remine000db -e REMINE_DB_USERNAME=redmine000kun REDMINE_DB_PASSWORD=rkunpass remine
```

### Redmine + MariaDB

```docker
docker network create redmine000net3

docker run --name mariadb000ex15 -dit --net=redmine000net3 -e MYSQL_ROOT_PASSWORD=mariarootpass -e MYSQL_DATABASE=redmine000db -e MYSQL_USER=redmine000kun -e MYSQL_PASSWORD=rkunpass mariadb --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci --default-authentication-plugin=mysql_native_password

docker run -dit --name redmine000ex16 --network redmine000net3 -p 8087:3000 -e REDMINE_DB_MYSQL=mariadb000ex15 -e REDMINE_DB_DATABASE=redmine000db -e REDMINE_DB_USERNAME=redmine000kun REDMINE_DB_PASSWORD=rkunpass redmine
```
