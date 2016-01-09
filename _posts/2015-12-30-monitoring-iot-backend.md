---
layout: post
title: "Limitless Garden - the home-grown IOT backend service"
description: "Monitoring the real world with Raspberry Pi and Arduino - part IV"
category: raspberry 
tags: [raspberry, arduino, iot, mongo, node.js, api]
---
{% include JB/setup %}

In the first three parts of this series we have looked into

- [part I](/raspberry/2015/07/18/Raspberry-Pi-Monitoring-With-Statsd/)
setting up a minimal monitoring system using a [statsd client in
python](https://pypi.python.org/pypi/statsd) on a [Raspberry Pi](https://www.raspberrypi.org/)

- [part II](/linux/2015/08/08/statsd-docker/) setting up a [statsd
server with graphite and grafana in a docker container](https://github.com/abarbanell/docker-grafana-graphite) on a small
[Azure](http://www.azure.com) server

- [part
III](http://blog.abarbanell.de/raspberry/2015/08/16/raspberry-arduino/) -
combining this with an [Arduino](http://www.arduino.cc) microcontroller to measure analog
data like the soil humidity of my plants

Now we want to go the next step toward an automated home irrgiation
system. For that we will need to go beyond visualization and anayse
the real data - so we need to save them first.

For this I have created a small IOT-backend written in node.js with
a MongoDB datastore. I could also have used one of the many cloud
offers already available for IOT data storage:

- [AWS IOT](https://aws.amazon.com/blogs/aws/aws-iot-cloud-services-for-connected-devices/)

- [Azure IoT Hub](https://azure.microsoft.com/en-us/services/iot-hub/)

- [adafruit.io](https://io.adafruit.com/)

- [thingspeak](https://thingspeak.com/)

- [thinger.io](https://thinger.io/)

- [initialstate.com](https://www.initialstate.com/)

- and may be many more.....

I have looked at them and then entered into a severe analysis paralysis - I could
not decide which one to use. So I thought it would be nice just to
start my own back end with a very simple CRUD code in node.js, the core api
code is just about 50-60 lines of code.

As a platform I have chosen heroku because it is so brilliantly
easy to get your environment assembled with a MongoDb storage, error
logging via papertrail, and just for fun I also added 3scale API
management so that I could control the traffic quota for each
application.

heroku chose also a name for my site which was such a good fit for
this internet-enabled home garden,  I just had to use this as the
project name and it is now on github as
[abarbanell/limitless-garden](https://github.com/abarbanell/limitless-garden).
I did not choose this name, the name chose this project by itself,
how could I refuse?

[![image of limitless garden website]({{ site.baseurl }}/assets/img/2015/12/lg.png)](https://github.com/abarbanell/limitless-garden)

This project has no user interface worth much at this time, although
this may come later. The only real meat is in  the [api functions](https://github.com/abarbanell/limitless-garden/blob/master/routes/api.js) which
allow you to save and retrieve arbitrary data. The api_key is the
most suitabe security for this as it is very lightweight - and we
are not talking about very sensitive data. We may need to revise
this if the data here could actually open some water pumps in your
home, you would not want this system to be compromised in this case.

## Conclusion

We have now build some monitoring and data capture system which
allows us to build further. the next iterations will be to scale
up the amount of data logged from our home garden, then build some
signals to water the plants, then push this signal down to automatically
operate a water pump.

Stay tuned for more.
