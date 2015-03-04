---
layout: post
category : raspberry
tagline: "getting Mono on the Raspberry"
tags : [raspberry, dotnet, mono]
---
{% include JB/setup %}


There two ways to get mono running on the Raspberry - just pulling
the repository package (easy, but you get an older version) or building it
yourself. I will quickly show both ways.

# Before you start

This was tried with a Raspberry type B (the [old
one](http://www.raspberrypi.org/products/model-b/)) and
[Raspbian](http://raspbian.org/). It is always good to do

```

$ sudo-apt-get update
$ sudo apt-get upgrade
```

before you start. It is also good to have the following

- the
[screen](http://www.rackaid.com/blog/linux-screen-tutorial-and-how-to/)
utility if you connect via ssh. We will have some
long-running builds and this keeps things running if your terminal
session disconnects.
- [git](http://git-scm.com/) - you need this to pull the sources. 
- for the second step building mono from sources you will need disk
space on you SD card - I will be trying it with 8GB after it failed
with a 4GB card.

# Easy way to get mono

To get some version mono installed just log in to your pi and do: 

```
$ sudo apt-get install mono-complete
```


This will take not too long and install mono version 3.2.8 which is easily verified: 

```
$ mono --version
Mono JIT compiler version 3.2.8 (Debian 3.2.8+dfsg-4+rpi1)
Copyright (C) 2002-2014 Novell, Inc, Xamarin Inc and Contributors. www.mono-project.com
	TLS:           __thread
	SIGSEGV:       normal
	Notifications: epoll
	Architecture:  armel,vfp+hard
	Disabled:      none
	Misc:          softdebug 
	LLVM:          supported, not enabled.
	GC:            sgen
```

you can then write a little sample program: 

```
$ vi HelloWorld.cs
```

with this content 

```
using System;
 
public class HelloWorld
{
    public static void Main()
    {
        Console.WriteLine("Hello World!");
    }
}
```

and compile and run like this: 

```
$ gmcs HelloWorld.cs
$ mono HelloWorld.exe
Hello World! 
```

Easy, but limited, we can do better.

# Build an up to date mono

*Make sure you have space on your disk - I have used a 8GB SD card*

We will have to get the sources and compile ourselves on the little
Raspberry - this will obviously take some time so we better let it
run overnight.

Why would you want to do this? Well, the killer argument is that the newest
ASP.NET 5 (vnext) will require mono 3.4.1 to start, and this is
where we will eventually go - running vNext on the Raspberry.

But first get a few tools...

```
$ sudo apt-get install autoconf
$ sudo apt-get install libtool
$ sudo apt-get install gettext
$ sudo apt-get install git
$ sudo apt-get install screen
```

If you have already done the easy install from packages in the first part of this article then
you are good, otherwise you will need a mono runtime to compile the
sources:

```
$ sudo apt-get install mono-runtime
```

And now we really start - take your time, the 
```
git clone
```
 takes minutes, the 
```
./autogen.sh
```
takes one hour  and
the 
```
make
```
takes several hours. The screen command is very helpful to make
sure your build runs even if you are logging in via ssh and your
network drops. The *mono* folder will be more than 1.5 GB size.

```
$ screen
$ git clone git://github.com/mono/mono.git
$ cd mono
$ ./autogen.sh --prefix=/usr/local
$ make 
$ sudo make install
```



