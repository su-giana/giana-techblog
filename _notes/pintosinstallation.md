---
title: "Pintos Installation (Using Docker)"
---

- From [[os]]

## 1. Pull Ubuntu Image from Docker

```shell
docker pull ubuntu:20.04
docker run -it --name pintos ubuntu:20.04 /bin/bash
```

## 2. Setting Up Ubuntu

```sh
apt update
apt-get install vim
apt-get install python3
apt-get install qemu
apt-get install gcc
apt-get install git
apt-get install sudo

git clone https://github.com/casys-kaist/pintos-kaist
```

Remember path to root directory of pintos project. Using ```pwd``` keyword let your work easier

```sh
vi ~/.bashrc

//put below line in the file
export PATH="$PATH:/pintos-kaist"

// reactivate the shell
source ~/.bashrc
```

```sh
cd {root directory of pintos project}
source ./activate
```

## 3. Run simple command

```sh
mkdir build
cd build
pintos -- run alarm-multiple > logfile
cat logfile
```