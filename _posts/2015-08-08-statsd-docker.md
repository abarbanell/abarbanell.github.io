---
category : linux
tagline: "install a statsd server with docker"
tags : [docker, statsd, azure, linux]
title: "Monitoring your Raspberry Pi with Statsd - Part II: the Statsd server"
---

In a previous post I have shown how to set up a 
[minimal server logging infrastructure on a Raspberry
Pi](/raspberry/2015/07/18/Raspberry-Pi-Monitoring-With-Statsd/)
with just a few (<25) lines of Python code.

In this part of the tutorial I will show how to set up the Statsd server to connect to. 

# Prerequisistes

I have chosen to set up a small cloud server on
[Azure](http://azure.microsoft.com/) - I have used a small Linux
server with

- Ubuntu 14.04 LTS
- size "Basic, A0" with 768 MB RAM
- you need to open the ports 80 (for the Grafana Website), 22 (for
ssh) and 8125/udp for Statsd. Please double check, Statsd is expecting
its data via UDP protocol, not TCP!

Once your server is up and running, you should log in via ssh and
add yourself to the docker group like so

```
$ sudo usermod -aG docker tobias
```
After this you need to close the connection and log in again for
this change to take effect.

You should verify that your installation works by running a simple
container, e.g.

```
$ docker run hello-world
```

# The docker-grafana-graphite container

There is a great docker container from here
[https://github.com/kamon-io/docker-grafana-graphite](https://github.com/kamon-io/docker-grafana-graphite])
which I took as a basis for my statsd server.

I have done the following changes to it for our purpose:

- changed the default dashboard to show
[RPYM](/raspberry/2015/07/18/Raspberry-Pi-Monitoring-With-Statsd/) data
instead of Kamon's welcome screen
- extended the data retention to have long term data up to 5 years
- changed the start script to have the data files from
/opt/graphite/storage/whisper in a volume outside of the docker
container for easier back up

you can get the changed dockerfile from
[my github repo](https://github.com/abarbanell/docker-grafana-graphite)
and run it with

```
$ ./start
```

this will pull the image from the docker hub and start it. There
will be two local folders for docker volumes, "./logs" and "./data".

<img src="/assets/img/2015/08/rpym-statsd.png" alt="RPYM data shown
with grafana, graphite, statsd" style="width: 100%;" />

# Conclusion

If you are now sending the RPYM data to the statsd server as described
[previously](/raspberry/2015/07/18/Raspberry-Pi-Monitoring-With-Statsd/)
then you can see all the nice Data from CPU usage, CPU temperature,
and also from the disk usage (added recently, after the first part
of this series of blog posts).

I hope you enjoy, let me know what you think.


