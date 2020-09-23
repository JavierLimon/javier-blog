---
layout: post
title: "Install Docker on Raspberry Pi 4"
---
![Docker Image](/assets/images/docker-image.png)

In this post we will learn how to install Docker in a raspberry pi 4

Requirements:
* Raspberry pi 4 with [Ubuntu 20.04.1 LTS]({{ 'https://ubuntu.com/download/raspberry-pi' | absolute_url }}){:target="_blank"} image.

First Uninstall old Versions of docker

```shell
sudo apt-get remove docker docker-engine docker.io containerd runc
```

This will preserve the content on /var/lib/docker/ images, containers, volumes and networks.

Install dependencies

```shell
sudo apt update && sudo apt install -y \
apt-transport-https ca-certificates curl \
gnupg-agent software-properties-common
```

Add Docker's official GPG key:

```shell
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
```

Set up the stable repository of Docker
```shell
sudo add-apt-repository "deb [arch=arm64] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable"
```
Update and install docker
```shell
sudo apt-get update && sudo apt install -y docker-ce docker-ce-cli containerd.io
```
Check the installation
```shell
sudo docker version
```
if you try to use the same command in the actual user(without sudo) you will get an error like this
```shell
 Version:           19.03.13
 API version:       1.40
 Go version:        go1.13.15
 Git commit:        4484c46
 Built:             Wed Sep 16 17:03:40 2020
 OS/Arch:           linux/arm64
 Experimental:      false
Got permission denied while trying to connect to the Docker daemon socket at
unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.40/version:
dial unix /var/run/docker.sock: connect: permission denied Client: Docker Engine - Community
```
To fix this this you need to add the current user to the [docker group]({{ 'https://docs.docker.com/engine/install/linux-postinstall' | absolute_url }})

add user to the docker group
```shell
sudo usermod -aG docker $USER
```
now apply the changes to the group
```shell
newgrp docker
```
now if you run docker version again you will get something like this
```shell
Client: Docker Engine - Community
 Version:           19.03.13
 API version:       1.40
 Go version:        go1.13.15
 Git commit:        4484c46
 Built:             Wed Sep 16 17:03:40 2020
 OS/Arch:           linux/arm64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.13
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       4484c46
  Built:            Wed Sep 16 17:02:11 2020
  OS/Arch:          linux/arm64
  Experimental:     false
 containerd:
  Version:          1.3.7
  GitCommit:        8fba4e9a7d01810a393d5d25a3621dc101981175
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
```
Congrats, Docker is working!

[Docker official install guide]({{ 'https://docs.docker.com/engine/install/ubuntu/' | absolute_url }}).
