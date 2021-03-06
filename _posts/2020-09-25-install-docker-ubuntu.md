---
layout: post
title: "Install Docker on Ubuntu"
categories: [Docker, Ubuntu, Linux]
---
In this post we will learn how to install Docker in a Ubuntu 20.04 Docker is the de facto standard to make containerized apps.

I need a way to practice and make further laboratories, and the best way to encapsulate the environment is with [Docker]({{'https://www.docker.com/why-docker' | absolute_url}}){:target="_blank"}.

* * *

![Docker Image](/assets/images/docker.jpg)

* * *

## [](#header-2)Requirements:


* [Ubuntu]({{ 'https://ubuntu.com/download' | absolute_url }}){:target="_blank"} image.
* Basic Linux commands
* Basic understanding of Docker

## [](#header-2)Hands on

If you are using Ubuntu with GUI desktop, please open a terminal to put the commands.

First we need to uninstall any older version of docker

This will preserve the content on /var/lib/docker/ images, containers, volumes and networks of any older version.

```shell
sudo apt-get remove docker docker-engine docker.io containerd runc
```

Install dependencies needed by docker.

```shell
sudo apt update && sudo apt install -y \
apt-transport-https ca-certificates curl \
gnupg-agent software-properties-common
```

Add Docker's official GPG key:

```shell
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
```

Set up the stable repository of Docker in our machine, you need to put the next command in a amd64 or x86 machine if you need the command for arm or armhf please go [here]({{'https://docs.docker.com/engine/install/ubuntu/' ! absolute_url}}){:target="_blank"} and grab the command from there.

```shell
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
$(lsb_release -cs) stable"
```
Update and install docker
```shell
sudo apt-get update && sudo apt install -y docker-ce docker-ce-cli containerd.io
```
You can check the installation with docker version, but you need to use sudo at the moment.
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
 OS/Arch:           linux/amd64
 Experimental:      false
Got permission denied while trying to connect to the Docker daemon socket at
unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.40/version:
dial unix /var/run/docker.sock: connect: permission denied Client: Docker Engine - Community
```
This is because you need to add the current user to the [docker group]({{ 'https://docs.docker.com/engine/install/linux-postinstall' | absolute_url }}){:target="_blank"}

```shell
sudo usermod -aG docker $USER
```
Now to this to work we need to apply the changes to the group, you can restart, or you can use the newgrp command
```shell
newgrp docker
```
You run docker version again you will get somethig like this
```shell
Client: Docker Engine - Community
 Version:           19.03.13
 API version:       1.40
 Go version:        go1.13.15
 Git commit:        4484c46
 Built:             Wed Sep 16 17:03:40 2020
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.13
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       4484c46
  Built:            Wed Sep 16 17:02:11 2020
  OS/Arch:          linux/amd64
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
Great, Docker is working!

References:

* [Docker official install guide]({{ 'https://docs.docker.com/engine/install/ubuntu/' | absolute_url }}){:target="_blank"}
*  [Docker post install guide]({{ 'https://docs.docker.com/engine/install/linux-postinstall' | absolute_url }}){:target="_blank"}

* * *
