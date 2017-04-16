---
title: "ASP.NET 5 (vNext) with docker on Linux"
category: dotnet
tags: [dotnet, tools, linux, docker]
---

In this post I show how to start working with the upcoming ASP.NET 5
(aka vNext) on Linux. At the time of this writing the latest version is
the beta 2 of ASP.NET 5 and in a short time you will be set up to run
ASP.NET in a docker container - no Windows is needed any more.

I have to give all the credit to Nikolai
Norman Andersen who showed this [setup for
MacOS](http://open.bekk.no/aspnet5-development-on-osx-with-docker),
this post is in large parts an adaptation to a Linux environment for
those people who do not work on MacOS.

# Overview

I will use a Linux desktop with Ubuntu 14.10 as a base, we need to go
through the following steps:

- get the docker software
- get a aspnet base image 
- set up your own project with yeoman

# Docker Installation

If you do not have docker installed already
you can find the details of the setup on the
[docker](https://docs.docker.com/installation/ubuntulinux/) website.

For the impatient here is a short step by step guide to install the
newest version of docker:

```
curl -sSL https://get.docker.com/ubuntu/ | sudo sh
```

verify the version and start the service if it has not come up
automatically:

```
tobias@tux03:~$ docker --version
Docker version 1.5.0, build a8a31ef

tobias@tux03:~$ sudo service docker status
docker stop/waiting

tobias@tux03:~$ sudo service docker start
docker start/running, process 11309

```

# Create a Base Image for ASP.NET

Here we have a lot of help from Microsoft which has
brought us a ready-made Dockerfile on their [github
site](https://github.com/aspnet/aspnet-docker/tree/master/1.0.0-beta2).

```
FROM mono:3.12

ENV KRE_VERSION 1.0.0-beta2
ENV KRE_USER_HOME /opt/kre

RUN apt-get -qq update && apt-get -qqy install unzip

RUN curl -sSL https://raw.githubusercontent.com/aspnet/Home/release/kvminstall.sh | KVM_BRANCH=v$KRE_VERSION sh
RUN bash -c "source $KRE_USER_HOME/kvm/kvm.sh \
  && kvm install $KRE_VERSION -a default \
  && kvm alias default | xargs -i ln -s $KRE_USER_HOME/packages/{} $KRE_USER_HOME/packages/default"

# Install libuv for Kestrel from source code (binary is not in wheezy and one in jessie is still too old)
RUN apt-get -qqy install \
  autoconf \
  automake \
  build-essential \
  libtool
RUN LIBUV_VERSION=1.0.0-rc2 \
  && curl -sSL https://github.com/joyent/libuv/archive/v${LIBUV_VERSION}.tar.gz | tar zxfv - -C /usr/local/src \
  && cd /usr/local/src/libuv-$LIBUV_VERSION \
  && sh autogen.sh && ./configure && make && make install \
  && rm -rf /usr/local/src/libuv-$LIBUV_VERSION \
  && ldconfig

ENV PATH $PATH:$KRE_USER_HOME/packages/default/bin
```

With this dockerfile we can build our image: 
```
sudo docker build -t aspnet .
```
Bingo, we now have a base image for our ASP.NEt project. 

# Set up your own Project with Yeoman

if you have no yeoman on your box you should quickly get it with 

```
sudo npm install -g yo
sudo npm install -g generator-aspnet
```

now you create you setup your project structure like this

```
mkdir myproj
cd myproj
yo aspnet
```

The yeoman tool will ask you a few questions about the type of your
project, just choose MVC as a start (or whatever other project type
you prefer).

Now create a Dockerfile for your application

```
FROM aspnet

ADD myproj /app/
WORKDIR /app

RUN kpm restore
EXPOSE 5004

ENTRYPOINT ["k", "kestrel"]
```

and build the image from this Dockerfile with
```
$ sudo docker build -t myproj .
```

Now you can run it with 

```
$ sudo docker run -i -p 5004:5004 -t myproj
```

Once you see the message 
```
Started...
```
your Kestrel webserver is running. you just connect with to 
```
http://localhost:5004
```

# Next Steps

I  would invite you to follow back on the original introduction at
[http://open.bekk.no/aspnet5-development-on-osx-with-docker](http://open.bekk.no/aspnet5-development-on-osx-with-docker) for some more explanation and 
how to map your local project folder into the container to make the updates and builds easier.

Here is a quick overview of the differences I found between the MacOs
and the Linux version:

- small differences in permissions as the docker installation and
execution of docker commands requires root (sudo) permissions where this
is not required on MacOS due to the fact that on Mac you actually run
the docker client on Mac while the docker engine runs in a Linux VM,
while on Linux both run on the same system.
- I have used a newer version of the
[Dockerfile](https://github.com/aspnet/aspnet-docker/tree/master/1.0.0-beta2)
as there are some improvements lately and the older version did not work
for me any more.
- networking differences as there is no need to map the docker daemon's
IP address to your local system, in Linux they both run on the same box
and connecting to your container only requires mapping of the TCP port,
not the IP address.






