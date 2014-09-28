---
layout: post
title: "Raspberry Pi and Akka / Scala"
description: ""
category: scala
tags: [scala, akka, raspberrypi]
---
{% include JB/setup %}

This post is about running [Akka](http://akka.io/) on the Raspberry Pi, but because this
is so simple to do I will also talk about a simpler development and
deployment process.

Clearly from my first [post](http:/2012/09/16/raspberry-pi-and-scala)
about Scala on the RaspberryPi you can see that the Java JVM on Raspbian
is not very fast, so I looked at a process of developing a Scala /
Akka application on my Linux desktop and just copy it to the Raspberry
Pi as a Jar file and execute there, to remove the tedious wait during
the compilation of my source code.


I used the solution [sbt-assembly](https://github.com/sbt/sbt-assembly)
plugin for sbt to achieve this.  It was
straightforward to create a single JAR (including the scala library and
everything) and then copy it over to the Raspberry Pi.

Here is how it goes:

On your desktop (Linux in my case) you should have the [Typesafe Stack]
(http://typesafe.com/stack)
installed, then go to your project directory and  create a new project
with this command (see details on the 
[Typesafe website](http://www.typesafe.com/resources/typesafe-stack/downloading-installing.html#akka) )

```sh
$ g8 typesafehub/akka-scala-sbt

# cd into the newly created directory and 
# run with 'sbt run'
```

Next you need to set up sbt-assembly as described on their 
[website](https://github.com/sbt/sbt-assembly),
this involves editing your project/plugins.sbt and ./build.sbt with a
few lines.

Then you can run

```sh
$ sbt run assembly
```

to create a big JAR file (about 10 MB) in the target subfolder and copy
to your Raspberry Pi with

```sh
$ scp HelloAkka-assembly-1.0.jar raspberry:/home/tobias
tobias@raspberry's password:
HelloAkka-assembly-1.0.jar 100% 10MB 5.2MB/s 00:02
```

then you log in on the Raspberry Pi and just execute it with

```sh
$ raspberry$ java -jar HelloAkka-assembly-1.0.jar
```

Now we have a nice simple process to copy any Scala application to the
Raspberry Pi and execute there, only the CPU speed and Memory size are
obvious limits.

