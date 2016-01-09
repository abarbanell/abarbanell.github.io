---
layout: post
title: "How to secure your arduino IOT network with Raspberry Pi and nginx"
description: ""
category: raspberry 
tags: [raspberrypi, arduino, nginx, iot, security]
---
{% include JB/setup %}

As a general rule, all API calls to your backend should always be
encrypted with https (SSL/TLS), this applies for IOT scenarios as
well as for internet traffic in general. However, if you use
cost-effective microcontrollers like Arduino or ESP8266, there is
not enough computing horsepower to encrypt the traffic, or even
enough memory to store the code for proper SSL/TLS handshake
algorithms. So you loose out or you need to move to more expensive
hardware.

This problem is listed as number 4 in the top 10 security vulnerabilities
for IOT on the [OWASP
website](https://www.owasp.org/images/8/8e/Infographic-v1.jpg), so
I did not want to send the data from my home sensors unencrypted
into the world.

Here I will describe a nice and elegant solution which solved this
problme for me.

# The story so far

In the previous parts of this series about Monitoring everything
with Raspberry Pi and Ardiuino I have described:

- [part I](/raspberry/2015/07/18/Raspberry-Pi-Monitoring-With-Statsd/)
setting up a minimal monitoring system using a [statsd client in
python](https://pypi.python.org/pypi/statsd) on a [Raspberry
Pi](https://www.raspberrypi.org/)

- [part II](/linux/2015/08/08/statsd-docker/) setting up a [statsd
server with graphite and grafana in a docker
container](https://github.com/abarbanell/docker-grafana-graphite)
on a small [Azure](http://www.azure.com) server

- [part
III](http://blog.abarbanell.de/raspberry/2015/08/16/raspberry-arduino/) -
combining this with an [Arduino](http://www.arduino.cc) microcontroller
to measure analog data like the soil humidity of my plants

- [part IV](/raspberry/2015/12/30/monitoring-iot-backend) - a
home-grown IOT-backend:
[limitless-garden](https://github.com/abarbanell/limitless-garden)

This is part V: how to secure the traffic between your microcontrollers
and the backend API, before I will in the next parts go into more details
about using Arduino microcontrollers with their own network access.

# Backend

In my case, I have my backend configured to accept only https
traffic. Any unencrypted http request is immediately redirected
with a 301 "Moved permanently" response, so that there is nothing
accepted without encryption.

# Raspberry Pi

In some of my use cases I have a Raspberry Pi to read some sensors,
or to connect to an Arduino via USB to use the Arduino's analog
input. This works without problem, because the Raspberry Pi (any
version) is strong enough to use https directly.

# Arduino and ESP8266

Some Arduino boards come with their own network connection. This
is either by adding an Internet or WiFi shield to an Arduino, or by
using a compatible board which has a Wifi chip on board, like
the [Wino board](http://www.wino-board.com) or the
[Photon](https://store.particle.io/collections/photon). Another
option is to use an [ESP8266](https://en.wikipedia.org/wiki/ESP8266)
 chip and directly program it. In all
of these cases you can send and receive data via Internet, so it would
be no problem to send your Sensor data with an HTTP post data to
an API endpoint.

Here is an example how to [read a soil humidity
sensor](https://github.com/abarbanell/arduino-wino/tree/master/sketch_soil_wifi)
 with a [Wino Board](http://wino-board.com) and send the data as
 JSON object to an API backend like
[limitless garden](https://github.com/abarbanell/limitless-garden).

But the traffic sent by the Arduino is not encrypted. I could accept
this on the WiFi network in my home, so as not to get paranoid about
people sniffing the traffic on my WiFi - but out on the internet?
No way. Compromising my backend server by letting it accept unencrytped
traffic? Surely not.

# Nginx to the rescue

[Nginx](http://nginx.org/en/) is a great piece of software and often used by
websites as a https frontend before your web server, to offload the
encryption work to a separate box. I figured out that this was
exactly the opposite from what I wanted to achieve.

[![nginx on Raspberry]({{ site.baseurl }}/assets/img/2016/01/nginx.png)](//https://github.com/abarbanell/limitless-garden)

Instead of sitting close to the web server and converting from https
to http, I wanted it to sit close to my web clients (my arduino's)
and convert from http to https. Then the requests could go out in
the world wide internet without being someone bad. So I quickly
reappropriated my oldes Raspberry Pi, a venerable model A, and told
it to

``` 
$ sudo apt-get install nginx 
```

then just adding one file in /etc/nginx/sites-available:

``` 
# proxy server for selected stuff from /api folder, 
# giving 404 on everything else

server {
	listen 8118; 
	location /api {
		proxy_pass      https://<<your-backend-server>>/api;
	} location / {
		return  404;
	}
} 
```

This actually only allows the api traffic to go out and not anything
else.

Now just activate this ruleset and deactivate the default (which
would be a local web server for static files):

```
$ sudo rm /etc/nginx/sites-enabled/default 
$ sudo ln -s /etc/nginx/sites-available/proxy /etc/nginx/sites-enabled 
$ sudo /etc/init.d/nginx restart 
```

Done. Now you can connect all of your arduinos to the port 8118
(or another one if you changed this in the proxy rules) and the
Raspberry will happily convert it to encrypted https traffic.

Please let me know in the comments whether this was helpful for
you.


