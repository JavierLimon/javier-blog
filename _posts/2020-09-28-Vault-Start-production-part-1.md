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

After we finish this post we going to review this points one by one, for now if you want to know more you can read it in the [Vault Production Hardening page]({{ 'https://learn.hashicorp.com/tutorials/vault/production-hardening' | absolute_url }}){:target="_blank"}.

## [](#header-3)In this post we will cover

* Download Binary from Hashicorp web page.
* Install the Vault in Ubuntu 20.04 LTS.
* Create the configurations files.
* Test the installation and web UI.

## [](#header-2)Requirements:

* Linux commands basic knowledge.
* Debian machine (In my case Ubuntu 20.04 aarch64)
* Basic Docker knowledge, you can see my other Docker post [here]({{ '/categories/#Docker' | absolute_url }}).





References:

* Hashicorp Vault [official install guide]({{ 'https://www.vaultproject.io/docs/install' | absolute_url }}){:target="_blank"}
* Hashicorp Vault [getting started guide]({{ 'https://learn.hashicorp.com/tutorials/vault/getting-started-install' | absolute_url}}){:target="_blank"}
* Vault [Production Hardening]({{ 'https://learn.hashicorp.com/tutorials/vault/production-hardening' | absolute_url }}){:target="_blank"}


## [](#header-1)This is a post in progress please check in 24 Hours.
