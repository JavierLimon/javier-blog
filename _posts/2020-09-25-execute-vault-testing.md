---
layout: post
title: "Install Hashicorp Vault for testing using Linux and Docker."
categories: [Vault, Docker, Linux]
---
In this post we going to install and run a vault server for testing, in mode developer.

* * *

![Vault Image](/assets/images/vault.png)

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

Now everything is working, lets start the vault in mode developer!

```bash
vault server -dev
```

You are going to see somthing like This

```bash
WARNING! dev mode is enabled! In this mode, Vault runs entirely in-memory
and starts unsealed with a single unseal key. The root token is already
authenticated to the CLI, so you can immediately begin using Vault.

You may need to set the following environment variable:

    $ export VAULT_ADDR='http://127.0.0.1:8200'

The unseal key and root token are displayed below in case you want to
seal/unseal the Vault or re-authenticate.

Unseal Key: j8i4C8CelXhbDN8AyUb0jUP40db7HhrvIMUTzeZV8Oc=
Root Token: s.EXiJeOOc838l848XrdxioI6Q

Development mode should NOT be used in production installations!

```

Every time you start the vault server on dev mode, you will get and Unseal Key, and a Root Token.

The Root token is the access to your vault server, always look for it, when you initialized.

Now the vault server is running in this terminal, connect to the docker container in a new terminal check the server.

```bash
vault status
```

and you will get an error "Error checking seal status: Get "https://127.0.0.1:8200/v1/sys/seal-status": http: server gave HTTP response to HTTPS client"

this is because we have not export the VAULT_ADDR, to do so, put the next command.

```bash
export VAULT_ADDR='http://127.0.0.1:8200'
```

and now when we check the status we will get something like This

```bash
root@60af5e1b7139:/# vault status
Error checking seal status: Get "https://127.0.0.1:8200/v1/sys/seal-status": http: server gave HTTP response to HTTPS client
root@60af5e1b7139:/# export VAULT_ADDR='http://127.0.0.1:8200'
root@60af5e1b7139:/# vault status
Key             Value
---             -----
Seal Type       shamir
Initialized     true
Sealed          false
Total Shares    1
Threshold       1
Version         1.5.4
Cluster Name    vault-cluster-8c2a9073
Cluster ID      240d9b52-07ed-148d-3395-ea0e4eb082a3
HA Enabled      false
```

Well done, your Vault test server is working. Remember Hashicorp Dev mode run in memory and all you do it will not be persistent, that means once you stop the server no data will be written.

for stop the server you can just do a control + C command in the vault running terminal.


References:

* Hashicorp Vault [official install guide]({{ 'https://www.vaultproject.io/docs/install' | absolute_url }}){:target="_blank"}
* Hashicorp Vault [getting started guide]({{ 'https://learn.hashicorp.com/tutorials/vault/getting-started-install' | absolute_url}}){:target="_blank"}
* Hashicorp [Vault Dev Mode]({{'https://www.vaultproject.io/docs/concepts/dev-server.html' | absolute_url}}){:target="_blank"}

* * *
