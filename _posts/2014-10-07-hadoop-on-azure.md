---
title: "Hadoop on Azure"
category: linux
tags: [dotnet, tools, hadoop, java ]
---

_(last updated 2014-10-11_)

## Rationale

For working with Hadoop I wanted a single-node hadoop server to try out
different ways of using Hadoop.

I had an existing [Azure](http://azure.microsoft.com/en-us/) subscription
so it worked out free for me, so I chose Azure to set up a small Linux
server with Hadoop.

So here we start - from 0 to Hadoop in 30 min.

## Azure basics - Create a server

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
1.75 GB memory)" size. If you just want to try out the setup then an A0
size server is enough, but it will run out of steam soon when you start
to run hadoop jobs on it, which you could resolve by tweaking the java
virtual machine memory configuration, but it is probably better to start
with a little more memory.

Let it automatically create a disk (storage account) and select a DNS
name of your choice, then create.

After a few minutes, you can log in to your new instance - replace the name of the key file and the dns name with your values:

```sh
$ ssh -i ~/.ssh/myPrivateKey.key yourdns.cloudapp.net
```
From start of server creation to first login should take you 
less than 10 minutes.

## Set up from a vanilla Ubuntu server 14.04

I like to have my setup scripts in a git repository so that I can
simply repeat the exercise. The following should also work on other
cloud providers like Amazon and Rackspace but I have only tested the
combnation Azure / Ubuntu 14.04.

Initially we need to do this first on a new server: 

```sh
$ sudo apt-get update
$ sudo apt-get upgrade
$ sudo apt-get install git-core
$ git clone https://github.com/abarbanell/hadoop-setup.git
$ cd hadoop-setup
$ . ./setup.sh
```

You should expect this to run  about 10 to 15 minutes from login to
finish of the setup script.

# Next Steps

This is not ready yet. Now the software is installed, but the hadoop
jobs are not yet configured. This needs to be added to the setup script
in the next version.

Here is a quick to-do list as a message to my future self:

- add your personal user account to group "hadoop"
- document how to: in Azure Portal, open port 8080 and  50070 for hadoop portal, with ACL for your IP.
- set up path so that hadoop jobs are more accessible
- create init script to start hadoop single node cluster on system reboot
- maybe create a skeleton work directory or scaffolding script for your
hadoop projects

# Validate everything is running

for now (without automatic start of the hadoop system at reboot) we can
verify that everyithing is working by following one of the examples from
the Hadoop distribution (I am assuming you are executing this with thea
same user account which you used above to run the setup script) :

```sh

$ cat 'export PATH=$PATH:/usr/local/hadoop/bin:/usr/local/hadoop/sbin'
$ start-dfs.sh
$ hdfs dfs -mkdir /user
$ hdfs dfs -mkdir /user/<your-user-id>
$ hdfs dfs -put /usr/local/hadoop/etc/hadoop input
$ hadoop jar \
/usr/local/hadoop/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.5.1.jar \
grep input out
$ hdfs dfs -get output output
$ cat output/*
$ stop-dfs.sh

```

If all of that worked then you can start with your own hadoop jobs.


## Optional stuff 

If you want to use git for your own projects on this server you should
configure your git identity with

```sh
$ git config --global user.name "Your Name"
$ git config --global user.email you@example.com
```

This also is very useful if you want to make changes to the setup script
and push these changes back to your own git repository.

# Reference

The commands inside the setup.sh ar largely based on the following references: 
- [Apache Hadoop instructions](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/SingleCluster.html)

