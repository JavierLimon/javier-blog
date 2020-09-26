---
layout: post
title: "Install Hashicorp Vault for testing using Linux and Docker."
categories: [Vault, Docker, Linux]
---
In this post we going to install and run a vault server for testing, vault had this mode called developer, so is going to be very easy to do.

Hashicorp offers Vault in Linux, Mac and Windows.

Hashicorp depending on the OS you can have one or more ways to installed, as it is always recommended to install manually in production, this is because you need a consistent version of vault and know everything is working properly, if someone upgrade the machine and change the vault binary with a newer version, some of the commands, or even it can loss the communication with other vault servers in case you have a cluster, so in this case is the method we going to use.

> If you update by accident Vault you can loss compatibility and connectivity with the cluster.

* * *

![Docker Image](/assets/images/vault.jpg)

* * *

> You need to remember that the Dev method in vault is only recommended for testing purposes and is more insecure and is not recommended by any way running for production.

## [](#header-2)Requirements:

* Linux commands basic knowledge.
* Debian machine (In my case Ubuntu 20.04 aarch64)
* Basic Docker knowledge, you can see my other Docker post [here]({{ '/categories/#Docker' | absolute_url }}).

## [](#header-2)Hands on

First we going to run a container with the latest ubuntu and we going to publish the port 8200 in the host for vault, if you want to know more about the working ports of Vault you can check it [here]({{'https://learn.hashicorp.com/tutorials/vault/reference-architecture#design-summary' | absolute_url}}){:target="_blank"}.

```shell
docker container run -it -d -p 8200:8200 --name ubuntu_vault_dev ubuntu
```

> We use the -p command to publish the port in the container to the host, this means that when we reach out the port 8200 in the host the docker container will respond.

now we need to conect to the container to install and run vault from there.

```bash
docker container exec -it ubuntu_vault_dev /bin/bash
```

now we need to go to

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
Well done!

References:

* Hashicorp Vault [official install guide]({{ 'https://www.vaultproject.io/docs/install' | absolute_url }}){:target="_blank"}
* Hashicorp Vault [getting started guide]({{ 'https://learn.hashicorp.com/tutorials/vault/getting-started-install' | absolute_url}}){:target="_blank"}

* * *
