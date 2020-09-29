---
layout: post
title: "Install Hashicorp Vault for production Part 2"
tagline: "Creating config files, starting the server and test UI"
categories: [Vault, Docker, Linux]
---
In this post we going to install and configure a vault server for production, this is the part 2.

The objective of this series is to gradually harden the vault server until we have a secure production cluster with the recommendations of Hashicorp.

* * *

![Vault Image](/assets/images/vault.png)

* * *

In the last post we see the list of recomendations from Hashicorp, you can check what we do so far.

## [](#header-3) Recomendations from Hashicorp.

* End-to-End TLS
* ~~Single Tenancy~~
* Firewall Traffic
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
* ~~Memory Lock in docker containers~~
* No Clear Text Credentials

And there is some recommendations if you are using Vault on Cloud.

After we finish this post of installation, we going to review this list one by one, for now if you want to know more you can read it in the [Vault Production Hardening page]({{ 'https://learn.hashicorp.com/tutorials/vault/production-hardening' | absolute_url }}){:target="_blank"}.

Beside this in the later post's I will also cover:

* High Availability (HA)
* Persistent and durable Backend.
* Logging.

## [](#header-3)In this post we will cover

* Cheking the installation
* Creating the config files for Vault.
* Create the configurations files
* Test the installation and web UI.

## [](#header-2)Requirements:

* Linux commands basic knowledge.
* A Debian computers (I'm using a raspberry pi 4 with Ubuntu 20.04)
* Basic Docker knowledge, you can see my other Docker's post [here]({{ '/categories/#Docker' | absolute_url }}).

## [](#header-2)Hands On:

Connect to the docker container with the exec command

```bash
docker container exec -it ubuntu_vault /bin/bash
```

Move to the working directory
```bash
cd /mnt/vault
```

Create the folders for config files and data in the /mnt/vault path and enter the directory

```bash
mkdir data config-files
```

```bash
cd config-files
```

Create a file named vault.hcl, edit the file and add the configuration for vault.

```bash
touch vault.hcl
```

```bash
nano vault.hcl
```

```bash
storage "file" {
  path = "/mnt/vault/data"
}
listener "tcp" {
  tls_disable = 1
  address     = "0.0.0.0:8200"
}
api_addr = "http://127.0.0.1:8200"
cluster_addr = "https://127.0.0.1:8201"
ui = true
```
> Tip, you can paste normally with control-V and then use Control-O to save, and Control-X to quit.
> If you need to know more command in nano you can visit the [Ubuntu Documentation]({{'https://help.ubuntu.com/community/Nano' | absolute_url}}){:target="_blank"}

Now the storage part is the backend where Vault will storage all his data, we are putting the data in /mnt/vault/data.
The tcp listener tells vault to respond from any network on port 8200, and the last part enables the Vault UI.

now lets start the server in production mode and pass the config folder

```bash
vault server -config=/mnt/vault/config-files/
```
You will get something like this

```bash
==> Vault server configuration:

             Api Address: http://127.0.0.1:8200
                     Cgo: disabled
         Cluster Address: https://127.0.0.1:8201
              Go Version: go1.14.7
              Listener 1: tcp (addr: "0.0.0.0:8200", cluster address: "0.0.0.0:8201", max_request_duration: "1m30s", max_request_size: "33554432", tls: "disabled")
               Log Level: info
                   Mlock: supported: true, enabled: true
           Recovery Mode: false
                 Storage: file
                 Version: Vault v1.5.4
             Version Sha: 1a730771ec70149293efe91e1d283b10d255c6d1

==> Vault server started! Log data will stream in below:

2020-09-29T20:59:05.391Z [INFO]  proxy environment: http_proxy= https_proxy= no_proxy=
```

Here you will check that The vault server is running and Memory Lock (MLock) is supported and enabled.

Now we need to export the VAULT_ADDR to vault know where is the responding address and port.

```bash
export VAULT_ADDR='http://127.0.0.1:8200'
```


In an other terminal connect to the docker container and lets initialized the vault server with vault operator init.

```bash
vault operator init

Unseal Key 1: +Qkda2AKtmWZYVJx/NnGt8a/rMdkaE82qvQn510J5knc
Unseal Key 2: ViPzHqmtdCMR6ckPwwf/JK9ULlKXTg8epVF3bvZz7c5L
Unseal Key 3: Eu0HSzz20pzBFKR8c6kUBVQNbq7j7LDJ+WIE/lNg1nmV
Unseal Key 4: s73B19sP9kS/RFOTkPKWCepgclZNL5hSvro3I9FsJ5J0
Unseal Key 5: PupDoVSUXsphQ2cwrn4O6CWDg92p2axVtpJXnWAZ36tA

Initial Root Token: s.pJdMkJuZ6em1Kvwk92ehN8C2

Vault initialized with 5 key shares and a key threshold of 3. Please securely
distribute the key shares printed above. When the Vault is re-sealed,
restarted, or stopped, you must supply at least 3 of these keys to unseal it
before it can start servicing requests.

Vault does not store the generated master key. Without at least 3 key to
reconstruct the master key, Vault will remain permanently sealed!

It is possible to generate new unseal keys, provided you have a quorum of
existing unseal keys shares. See "vault operator rekey" for more information.
```

If everything is ok you will get the unseal Keys and the Root Token, this is the only time this information will be on screen so make sure to save it temporarily until we finish the lab, as Hashicorp do not recommend to store all the unseal keys in the same place.

if you do a vault status you will see the vault server is sealed, you need to unseal it to work, you can read more at [Hashicorp Seal/Unseal]('https://www.vaultproject.io/docs/concepts/seal.html' | absolute_url){:target="_blank"}

now lets unseal the vault server.

repeat this command three times and putting a different Unseal Key each time, it only need three to unlock generate the master key and unlock the vault data.

```bash
vault operator unseal
```

> Remember that every time the vault server is started it will start Seal, if you want to know more of how to AutoUnseal you can read more at [Hashicorp AutoUnseal]({{'https://learn.hashicorp.com/collections/vault/auto-unseal' | absolute_url}}){:target="_blank"}

at the third time you put the Unseal Key's you will get

```bash
Key             Value
---             -----
Seal Type       shamir
Initialized     true
Sealed          false
Total Shares    5
Threshold       3
Version         1.5.4
Cluster Name    vault-cluster-e32c8c61
Cluster ID      7f484d64-65c8-204b-c1db-239dee0eac26
HA Enabled      false
```

Well done now we have our vault server running and unseal, now lets test using the root token and the ui.

If you try to do any operations that will require admin access like listing the auth methods, you will get an error.

```bash
vault auth list

Error listing enabled authentications: Error making API request.

URL: GET http://127.0.0.1:8200/v1/sys/auth
Code: 400. Errors:

* missing client token
```

that because you havent use the root token, in dev vault automaticatly use the root token for every petition, in production you need to specify by exporting the VAULT_TOKEN variables, remember to change the token and use yours.

```
export VAULT_TOKEN=s.pJdMkJuZ6em1Kvwk92ehN8C2
```

After this you have root permissions.

```bash
vault auth list
Path      Type     Accessor               Description
----      ----     --------               -----------
token/    token    auth_token_ceb73f6e    token based credentials
```

> Is not recommended to use the Root Token, instead is recommended to add tokens with administrative access and revoke the root token. we going to see this in next posts.

Now lets check the UI, you can use your root token to temporarily access.

you only need to connect to the computer IP in your browser in my case is http://192.168.1.50:8200/ui remeber to put the 8200 that is the port Vault will respond.

![Vault UI](/assets/images/post/vault-login.png)

login with your root token.

![Vault UI](/assets/images/post/vault-ui.png)

Well done, you finish the Part 2!

In the next post we going to create a service to run vault server on start in the docker container.

## [](#header-2)To clean up.
You can press control - C the first terminal window (where vault server is running) to stop the server, and stop the container.

References:

* Hashicorp Vault [official install guide]({{ 'https://www.vaultproject.io/docs/install' | absolute_url }}){:target="_blank"}
* Hashicorp Vault [getting started guide]({{ 'https://learn.hashicorp.com/tutorials/vault/getting-started-install' | absolute_url}}){:target="_blank"}
* Vault [Production Hardening]({{ 'https://learn.hashicorp.com/tutorials/vault/production-hardening' | absolute_url }}){:target="_blank"}
* Hashicorp Vault [Server Configuration]({{ 'https://www.vaultproject.io/docs/configuration' | absolute_url}}){:target="_blank"}
* [Hashicorp Seal/Unseal]('https://www.vaultproject.io/docs/concepts/seal.html' | absolute_url){:target="_blank"}
* [Hashicorp AutoUnseal]({{'https://learn.hashicorp.com/collections/vault/auto-unseal' | absolute_url}}){:target="_blank"}


## [](#header-1)This is a post in progress please check in 24 Hours.
