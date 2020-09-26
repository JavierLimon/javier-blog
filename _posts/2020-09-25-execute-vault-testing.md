---
layout: post
title: "Install Hashicorp Vault for testing using Linux and Docker."
categories: [Vault, Docker, Linux]
---
In this post we going to install and run a vault server for testing, in mode developer.

* * *

![Docker Image](/assets/images/vault.png)

* * *

Hashicorp offers Vault in Linux, Mac and Windows.

Vault, depending on the OS, have one or more ways to be installed, it is always recommended to install manually in production. This is because you need a consistent version of vault to know everything is working properly, if someone upgrade the vault binary with a newer version, is possibly some of the commands can stop to work, or even you can loss the communication with vault cluster. We going to use the manually install.

> If you update by accident Vault in production, you can loss compatibility and connectivity with the cluster.

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



## [](#header-1) Post on progress, please check it in 24 hours..


References:

* Hashicorp Vault [official install guide]({{ 'https://www.vaultproject.io/docs/install' | absolute_url }}){:target="_blank"}
* Hashicorp Vault [getting started guide]({{ 'https://learn.hashicorp.com/tutorials/vault/getting-started-install' | absolute_url}}){:target="_blank"}

* * *
