---
layout: post
category : raspberry
tagline: "Raspberry Pi Monitoring"
tags : [raspberry, monitoring, linux, statsd]
title: Monitoring your Raspberry Pi with Statsd - Part I
---
{% include JB/setup %}


I like to toy with my Raspberry - as evidenced by other
[posts on this blog](/categories.html#raspberry-ref). 

But running any server, however small, without proper monitoring
is just not what I can do - its against the grains of my work ethics
in some way. Especially if the number of Rasperry boxes starts to grow.

Also, one of my next projects will require the logging of external
sensor data, and a general purpose monitoring infrastructure is a
good start for that.

# Design Decisions

I had some experience with a variety of monitoring systems (Nagios,
New Relic,  Statsd are the more recent experiences) and had a
preference for something I knew.

In this scenario I wanted the following points to be covered: 

- I wanted to run the graphs from a server instead of the Raspberry,
because I wanted to monitor multiple Rasperry Pi's, and not spoil
one of them with a web frontened and data store for the monitoring
itself.
- I wanted something where an application can send arbitrary metrics
without previous configuration on the server.  Not a polling system
like in Nagios, but a push based system like New Relic or Statsd.

New Relic looks fine at first, but the "arbitrary metrics" part is
only available in the paid version, so this is out.

Decision taken: I will be logging via statsd. A server is quickly
set up as a docker container on a server somewhere (I have put it
on Azure for now), an example is described
[here](https://github.com/kamon-io/docker-grafana-graphite) - but
I may go into more details of the statsd server setup in another
post later.

# Prerequisites

For now we just need to have a running statsd instance, reachable
on the network on the standard statsd port 8125 - for covenience
we put this IP address into our /etc/hosts file on the Raspberry:

```
192.168.100.200 statsd
```

Of course your IP address will be different.

Now we can just go on the Raspberry and do: 

```
sudo apt-get install python-pip
```

and then we can install the 
[Python Statsd Client](https://pypi.python.org/pypi/statsd) with 

```
$ pip install statsd
```

and then send our first metric with the python commands: 

```
>>> import statsd
>>> c = statsd.StatsClient('statsd', 8125)
>>> c.incr('foo')  # Increment the 'foo' counter.
```

# Monitoring CPU

The first thing to monitor is always the CPU usage. The python package 
psutil will do the trick. So we just need to write a little python script
and call it from crontab every minute. 

mon.py: 

```
import statsd
import os
import psutil

host = os.uname()[1]
cpu = psutil.cpu_percent(interval=1)

c = statsd.StatsClient('statsd', 8125, prefix=host)

c.incr('heartbeat')

c.gauge('cpu.percent', cpu)
```

# GIT install

You can find all of this on github in the project
[rpym](https://github.com/abarbanell/rpym). It also has a setup
script to download all the necessary python libraries and ubuntu
packages.

So then you just need to do three steps: 

- set up /etc/hosts for your statsd server
- get the monitoring script
- call your monitoring script from crontab

/etc/hosts:

```
# change to your statsd server IP address
192.168.100.200 statsd
```

GIT install of the monitoring script: 

```
git clone https://github.com/abarbanell/rpym.git
cd rpym 
./setup.sh
```

you will be asked for the sudo password during installation.


Crontab:

```
# m h  dom mon dow   command
# change the path as necessary to point to the git repository on your system
* * * * * $HOME/github/abarbanell/rpym/mon.py
```

# Results

This was just a first step to get the monitoring infrastructure in
place. From here on it will be straightforward to add more metrics
later.

Meanwhile, I can enjoy the beauty of the combined CPU graphs of my
three Raspberry Pi boxes.

<img src="/assets/img/2015/07/18/cpu-monitor.png" alt="CPU Monitoring" style="width: 100%;" />

Stay tuned for more.

## Added 19-Jul-2015: CPU temp reading

As mentioned above adding more metrics will be easy, as a first
exercise I have added CPU temperature readings. If you pull the
lates from the [rpym git repo](https://github.com/abarbanell/rpym)
then you get this as well sent to your statsd server.

