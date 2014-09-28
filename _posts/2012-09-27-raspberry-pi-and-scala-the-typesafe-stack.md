---
layout: post
title: "Raspberry Pi and Scala: The Typesafe Stack"
description: ""
category: scala
tags: [raspberrypi, scala]
---
{% include JB/setup %}

Following on from my earlier
[post](http:/2012/09/16/raspberry-pi-and-scala/) I wanted to get the
the full [Typesafe Stack](http://typesafe.com/stack) on my little Raspberry Pi.

So here we go:

according to the documentation on the [Typesafe website](http://typesafe.com/stack/download#universal)
 you should use the “Universal install”, i.e. download a tarball and copy it to a place of your choice.

```sh
$ wget http://downloads.typesafe.com/typesafe-stack/2.0.2/typesafe-stack-2.0.2.tgz
$ tar xovzf typesafe-stack-2.0.2.tgz
$ sudo mv typesafe-stack /opt
$ chown -R bin.bin /opt/typesafe-stack
```

Next you need to make sure the new software is found by adding the bin
directory of the typesafe stack to your PATH (and if you followed my
earlier post you should also remove the separate sbt installation from
your PATH)

my /etc/profile now contains this:

```sh
$ PATH=$PATH:/opt/typesafe-stack/bin
$ export PATH
```

then log out and log in again and check your PATH.

```sh
$ type sbt
$ type g8
```

these should point to the newly installed typesafe-stack directory.

You can now either run your commands with reduced memory allocation with
the -mem 128 flag or you can hack the startup file:

```sh
$ diff sbt-launch-lib.bash sbt-launch-lib.bash.orig
79c79
< local mem=${1:-128}
—
> local mem=${1:-1536}
```

Now we go into our project directory (e.g. $HOME/proj or whatever you choose) and do

```sh
$ g8 typesafehub/scala-sbt
```

this will ask for project name and a few other things and create a directory for the new project

```sh
$ cd <your new project directory>
$ sbt -mem 128 run
```

Now after a fair while you will see the code compile and run.

