---
layout: post
category : raspberry
tagline: "getting Mono on the Raspberry"
tags : [raspberry, dotnet, mono]
---
{% include JB/setup %}


There three ways to get mono running on the Raspberry - just pulling
the raspbian repository package (easy, but you get an older version), 
pulling it from a repository of the mono project,  or building it
yourself. I will quickly show each way.

# Before you start

It is always good to do

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
- for the third step building mono from sources you will need disk
space on you SD card - I will be trying it with 8GB after it failed
with a 4GB card.

# Easy way to get mono

This was tried with a Raspberry type B (the [old
one](http://www.raspberrypi.org/products/model-b/)) and
the new model 
[Raspberry 2 Model B ](http://www.raspberrypi.org/products/raspberry-pi-2-model-b/),
in both cases with [Raspbian](http://raspbian.org/). 


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

# Get an up to date mono

A newer version of Mono is available from the repository of the mono project with
the following commands:

```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF 
echo "deb http://download.mono-project.com/repo/debian wheezy main" | sudo tee /etc/apt/sources.list.d/mono-xamarin.list 
sudo apt-get update 
sudo apt-get upgrade 
sudo apt-get install mono-complete
```

As usual, there is a catch - the latest version of it is compiled for the ARM7 processor so it won't work on the old Raspberry Model 1. Luckily my new Model 2 just arrived, so we are back in business.

If all goes well you should see this: 

```
$ mono --version
Mono JIT compiler version 3.12.0 (tarball Sat Feb  7 19:30:32 UTC 2015)
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

If you have a Raspberry 2 then this is by far the preferred way.

# Third option - build from sources. 

We will have to get the sources and compile ourselves on the little
Raspberry - this will obviously take some time so we better let it
run overnight.

Even worse - there is a catch: to compile this you need a recent version 
of mono already installed, e.g. to compile the most current version you need at least
mono 3.8, but the Raspbian repository only has 3.2.8, so without
the package from our second step you cannot actually do this.

Why would you want to anyway? Well, the killer argument is - just because you can. 
There really is no other point in it. So proceed or skip this as you prefer.

You are still reading, so you have decided to go ahead. Good. 

*Make sure you have space on your disk - I have used a 8GB SD card
and the build folder will be about 2GB at the end of this.*

Then first get a few tools...

```
$ sudo apt-get install autoconf libtool gettext git screen
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
takes several hours. The 
```
screen 
```
command is very helpful to make
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

As usual we check afterwards what we got now: 

```
$ mono --version
Mono JIT compiler version 4.1.0 (master/4f458a4 Fri Mar  6 23:35:49 CET 2015)
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

In the first line it gives the time and source of the build, here it is "(master/4f458a4 Fri Mar  6 23:35:49 CET 2015)",
the version number 4.1.0 looks strange as the [newest released version](http://www.mono-project.com/docs/about-mono/releases/) 
is 3.12.0 at this time .

Anyway. We now the newest Mono now.

## Conclusion

That's all for now, we have seen three ways of installing Mono on the Raspberry Pi, and we are now ready, to look for
a good us of the mono environment in a future post on this blog.






