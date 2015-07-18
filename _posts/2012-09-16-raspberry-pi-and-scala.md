---
layout: post
title: "Raspberry Pi and Scala"
description: ""
category: raspberry
tags: [raspberry, scala]
---
{% include JB/setup %}

This is a little experience report how to bring up [Scala](http://www.scala-lang.org/) 
on my [RaspberryPi](http://www.raspberrypi.org/)

[![Raspberry Pi Image](/assets/img/2012/09/200px-wpid-13490104119771.jpg)]
(/assets/img/2012/09/wpid-13490104119771.jpg)

## Step 1: Java

I have connected my Raspberry only with power and network (no
keyboard/mouse/display) for now and log in via ssh from my Linux
desktop. Not only to save cables on the desk, but also to allow copy
and paste between desktop and RaspberryPi. I am running Raspbian wheezy
2012-08-16 downloaded from the Raspberry website.

Java is installed quick and easy with:

```sh
$ sudo apt-get install openjdk-7-jdk
```


This pulls all its prerequisites and takes a few minutes. Next I
was curious to check and just took a little test program like this
“hello.java”

```java
class hello
{  
        public static void main(String args[])
        {
           System.out.println("Hello World!");
        }
}
```

Just compile with

```sh
$ javac hello.java
```

and run with

```sh
$ java hello
```

Curious about performance I took some measurements, first measuring the
speed of compilation for a minimal program like this:

```sh
$ time javac hello
```

results: 14 sec. Then I tried to measure the memory size and added a sleep in the program like this:

```java
class hello
{
  public static void main(String args[])
  {
    System.out.println("Hello World!");
    try {
      Thread.sleep(10000); // 10 sec
    } catch (Exception e) {
    }
  }
}
```

and run with

```sh
$ java hello &
$ ps u
```

Result: Virtual Size (VSZ): 215 MB, Resident Size (RSS): 9MB

## Step 2: set up sbt

download with

```sh
$ wget http://scalasbt.artifactoryonline.com/scalasbt/sbt-native-packages/org/scala-sbt/sbt/0.12.0/sbt.tgz
```

unpack and copy to a directory of your choice I have used

```sh
$ tar tvozf sbt.tgz
$ mv sbt $HOME
```

The sbt directory contains a “bin” directory which needs to go in your path by adding this to your $HOME/.bashrc:

```sh
$ export PATH=$HOME/sbt/bin:$PATH
```

open a new shell to reload .bashrc and execute

```sh
$ sbt -mem 128 -help
```

you should see a help message. The parameter -mem 128 will allow 128
MB for the JVM, the default of 1536 MB is way too big for the little
Raspberry Pi.

Alternatively you can put a file .sbtopts in your current directory with
the sbt command line switches required, in ths case “-mem 128″.

Now, when starting

```sh
$ sbt
```

it will pull some files and updates, slowly, and after some minutes… The sbt prompt comes up fine.

Next I created a small “Hello world” program like this in “hello.scala”:

```scala
package greeter
object Hello extends App {
  println("Hello, World!")
}
```

and started with

```sh
$ sbt run
```

this compiles the scala and java files in your directory – this takes a fair while  – and then success:

```
[info] Running greeter.Hello
Hello, World!
[success] Total time: 507 s, completed 16-Sep-2012 19:02:03
```

Wow, over 8 minutes for this little piece. But it worked.

## What next? 

I have seen some discussion on the web that the Oracle JVM may be faster,
but also not compatible with Raspbian, so this will be a topic for further
investigation. For now it looks like we have to wait for Oracle to release
a compatible version, or choose another OS for Raspberry, one alternative
approach using Debian Wheezy instead of Raspbian Wheezy can be found here:
[
http://www.savagehomeautomation.com/projects/raspberry-pi-installing-oracle-java-development-kit-jdk-170u.html
](
http://www.savagehomeautomation.com/projects/raspberry-pi-installing-oracle-java-development-kit-jdk-170u.html
)

