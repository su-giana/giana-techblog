---
title: "Communication of Container"
---

- From [[docker]]

## Communication of Container
**Port** setting is needed to access a container through web brower. Physical server(computer) has to receive access of approch from outside of container using **port** and deliver it to container's **port** like **"-p {host port}:{continer port}"**.
For instance,

```
docker run --name {container name} -d  -p 80:80 httpd
```

## Generation of Container
- Setting  **root password** is mandatory if you create database management software with **-e {}_ROOT_PASSWORD={password} option**
- Docker official image includes runtime and software for executing program

## Deletion of image
### Delete image

```
docker image rm {image_name}
```
