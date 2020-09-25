---
layout: post
title: "Run containers with Docker"
categories: Docker
---
In this post we going to learn how to run docker images, options and commands related to it.

![Docker Image](/assets/images/run-docker.png)

* * *

## [](#header-2)Requirements:

* Have Docker installed.

## [](#header-2)Hands on

To run docker images you can use the command container run

```shell
docker container run -it -d --name my_container image_name command
```

For example

```bash
docker container run -it -d --name ubuntu_dev ubuntu /bin/bash
```

## [](#header-2)Ok lets explain all the options we have here.

* The -i option means that it will be interactive mode (you can enter commands to it)
* The -t option gives you a terminal (so that you can use it as if you used ssh to enter the container).
* The -d option (daemon mode) keeps the container running in the background.
* --name my_container name of the container.
* image_name docker will try to find the image locally at first if does not exist it will search for the image in [docker hub]({{ 'https://hub.docker.com/' | absolute_url }}){:target="_blank"}.
* /bin/bash is the command it runs as pid 1, if this command stop all the container stops (Not required).

> Docker Hub is an image repository where you can find official and unofficial docker images.

Once we put the last command docker will download and run the latest ubuntu image from docker hub.

```bash
$ docker container run -it -d --name dev_ubuntu ubuntu /bin/bash
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
33c9bf8777b5: Pull complete
3a1900e16211: Pull complete
d53c8a81c254: Pull complete
Digest: sha256:cbcf86d7781dbb3a6aa2bcea25403f6b0b443e20b9959165cf52d2cc9608e4b9
Status: Downloaded newer image for ubuntu:latest
91773f80517d8c9f7777db9ea651e48f993d9eb66bf6b0e5bf6240845308c05e
```

Once it finish you can verify that the container is running
```bash
$ docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
91773f80517d        ubuntu              "/bin/bash"         2 minutes ago       Up 2 minutes                            dev_ubuntu
```

Now lets connect to the container with container exec
```bash
docker container exec -it dev_ubuntu /bin/bash
```
Once you connect to the container you can update, or do what ever you want with the container, and use exit for logout.

```bash
$ exit
```

Well now that you know how to run containers you can look on Docker Hub, where you can find almost infinite containers for all purpose, samba, web, dns, dhcp have fun!

Now, lets clean up.

First we need to stop the container.

```bash
docker container stop dev_ubuntu
```

If you want to start the container later you can use the command start

```bash
docker container start dev_ubuntu
```

But in this case we going to remove the container after stopping with rm.

```bash
docker container rm dev_ubuntu
```
And then delete the ubuntu image

```bash
docker image rm ubuntu:latest
```

Well done, now you know how to run a container with Docker.
