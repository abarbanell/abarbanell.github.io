---
layout: post
title: "Hadoop on Azure"
description: ""
category: linux
tags: [.NET, Tools, hadoop, java ]
---
{% include JB/setup %}

## Rationale

For working with Hadoop I wanted a single-node hadoop server to try out
different ways of using Hadoop.

I had an existing [Azure](http://azure.microsoft.com/en-us/) subscription
so it worked out free for me, so I chose Azure to set up a small Linux
server with Hadoop.

## Azure basics

If you do not have an Azure account, you can get one [here]
(http://azure.microsoft.com/en-us/).

To create a server you need to first create a ssh certificate
unless you want to give your password each time.

The Linux procedure is documented in more detail
[here](http://azure.microsoft.com/en-us/documentation/articles/virtual-machines-linux-use-ssh-key/)
but for me on my existing linux desktop it was just:

```sh
$ openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
-keyout myPrivateKey.key -out myCert.pem
$ chmod 600 myPrivateKey.key
```

Then go to the [Azure portal](https://manage.windowsazure.com) and spin
up a linux server with Ubuntu 14.04 LTS in the "Standard_A1 (1 core,
1.75 GB memory)" size.

Let it automatically create a disk (storage account) and select a DNS
name of your choice, then create.

After a few minutes, you can log in to your new instance - replace the name of the key file and the dns name with your values:

```sh
$ ssh -i ~/.ssh/private.key yourdns.cloudapp.net
```
From start of server creation to first login should take you 
less than 10 minutes.

## Set up from a vanilla Ubuntu server 14.04

I like to have my setup scripts in a git repository so that I can
simply repeat the exercise. The following should also work on other
cloud providers like Amazon and Rackspace.

Initially we need to do this first on a new server: 

```sh
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get install git-core
$ git clone https://github.com/abarbanell/hadoop-setup.git
$ cd hadoop-setup
$ ./setup.sh
```

You should expect this to run  about 10 to 15 minutes from login to
finish of the setup script.

# Next Steps

This is not ready yet. Now the software is installed, but the hadoop
jobs are not yet configured. This needs to be added to the setup script
in the next version.

# Optional steps

If you want to use git for your own projects on this server you should
configure your git identity with

```sh
$ git config --global user.name "Your Name"
git config --global user.email you@example.com
```

# Reference

The commands inside the setup.sh ar largely based on the following references: 
- [Apache Hadoop instructions](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/SingleCluster.html)

