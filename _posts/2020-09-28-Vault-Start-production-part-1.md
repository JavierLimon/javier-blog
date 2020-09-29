---
layout: post
title: "Install Hashicorp Vault for production Part 1."
categories: [Vault, Docker, Linux]
---
In this post we going to install and configure a vault server for production, this is the part 1.

The objective of this series is to gradually harden the vault server until we have a secure production cluster with the recommendations of Hashicorp.

* * *

![Vault Image](/assets/images/vault.png)

* * *

First we want to know what are the recommendations from Hashicorp for a secure Vault server.

## [](#header-3)Recomendations from Hashicorp.

* End-to-End TLS
* Single Tenancy
* Firewall Trafic
* Disable SSH / Remote Desktop
* Disable Swap
* Don't run as Root
* Turn Off Core Dumps
* Immutable Upgrades
* Avoid Root Tokens
* Enable Auditing
* Upgrade Frequently
* Configure SELinux / AppArmor
* Restrict Storage Access
* Disable Shell Command History
* Tweak Limits
* Docker Containers (memory Lock)
* No Clear Text Credentials

And there is some recommendations if you are using Vault on Cloud.

Beside this it is also recommended that your vault server have High Availability (HA) and had a persistent and durable Backend.

After we finish this post of installation, we going to review this list one by one, for now if you want to know more you can read it in the [Vault Production Hardening page]({{ 'https://learn.hashicorp.com/tutorials/vault/production-hardening' | absolute_url }}){:target="_blank"}.

## [](#header-3)In this post we will cover

* Download Binary from Hashicorp web page.
* Install Vault in a Docker container (Ubuntu 20.04 LTS).
* Create the configurations files.
* Test the installation and web UI.

## [](#header-2)Requirements:

* Linux commands basic knowledge.
* Debian machine (In my case Ubuntu 20.04 aarch64)
* Basic Docker knowledge, you can see my other Docker post [here]({{ '/categories/#Docker' | absolute_url }}).

First we going to run a container with the latest ubuntu and we going to publish the port 8200 in the host for vault, if you want to know more about the working ports of Vault you can check it [here]({{'https://learn.hashicorp.com/tutorials/vault/reference-architecture#design-summary' | absolute_url}}){:target="_blank"}.

```shell
docker container run -it -d --name ubuntu_vault_dev ubuntu /bin/bash
```

We need to conect to the container to install and run vault from there.

```bash
docker container exec -it ubuntu_vault_dev /bin/bash
```

Now we need to install some tools we going to need later wget and unzip.

```bash
apt update && apt install wget unzip -y
```

> When Im using commands in the containers shell is posible that you need to use the command sudo in front of your commands if you are not using a container, example: "sudo apt update && sudo apt install wget -y"

Now we need to know what architecture you are working on, you can use uname -m to check it.

```bash
uname -m
aarch64
```
In my case as I'm running the image in a Raspberry pi 4, my architecture is aarch64 (arm64).

Now we going to go to the Hashcorp Vault download page [here]({{ 'https://www.vaultproject.io/downloads' | absolute_url }}){:target="_blank"} and we need to select the right binary and architecture to download.

![Docker Image](/assets/images/post/2020-09-25-download1.png)

And insted on hitting the Download button do a right click and select "Copy Link Location".

![Docker Image](/assets/images/post/2020-09-25-download2.png)

Now in you have everything to install in the container, we need to download the binary in the container, run the command wget and paste the Link.

```bash
wget https://releases.hashicorp.com/vault/1.5.4/vault_1.5.4_linux_arm64.zip
```
Next Unzip the file

```bash
unzip vault_1.5.4_linux_arm64.zip
```
And delete the zip file

```bash
rm vault_1.5.4_linux_arm64.zip
```

Lets test the vault binary

```bash
./vault version
Vault v1.5.4 (1a730771ec70149293efe91e1d283b10d255c6d1)
```
Now lets move the binary to a environment path, to be able to execute everywhere use the command echo $PATH to know that are the environment variables.

```bash
root@60af5e1b7139:/# echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

Now move the vault binary to a PATH enviroment folder, personally I prefer /usr/local/bin/ because is almost empty (for testing).

```bash
mv vault /usr/local/bin/

```

Ok! we are almost there, lets check the installation.

```bash
vault version
Vault v1.5.4 (1a730771ec70149293efe91e1d283b10d255c6d1)
```



References:

* Hashicorp Vault [official install guide]({{ 'https://www.vaultproject.io/docs/install' | absolute_url }}){:target="_blank"}
* Hashicorp Vault [getting started guide]({{ 'https://learn.hashicorp.com/tutorials/vault/getting-started-install' | absolute_url}}){:target="_blank"}
* Vault [Production Hardening]({{ 'https://learn.hashicorp.com/tutorials/vault/production-hardening' | absolute_url }}){:target="_blank"}


## [](#header-1)This is a post in progress please check in 24 Hours.
