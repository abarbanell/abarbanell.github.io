---
layout: post
category : raspberry
tagline: "Connecting an Arduino to a Raspberry"
tags : [raspberry, arduino, linux, monitoring, iot]
title: "Monitoring the Real World with your Raspberry Pi - Part III: connecting an Arduino "
---
{% include JB/setup %}

This is a followup on my recent blog posts about monitoring 
my fleet of Raspberry Pi's with statsd and a very 
simple monitoring script on the Raspberry. Please check the following posts about this series:

- [part I](/raspberry/2015/07/18/Raspberry-Pi-Monitoring-With-Statsd/)
setting up a minimal monitoring system using a [statsd client in
python](https://pypi.python.org/pypi/statsd) on a [Raspberry
Pi](https://www.raspberrypi.org/)

- [part II](/linux/2015/08/08/statsd-docker/) setting up a [statsd
server with graphite and grafana in a docker
container](https://github.com/abarbanell/docker-grafana-graphite)
on a small [Azure](http://www.azure.com) server

- [part
III (this blog post)](http://blog.abarbanell.de/raspberry/2015/08/16/raspberry-arduino/) -
combining this with an [Arduino](http://www.arduino.cc) microcontroller
to measure analog data like the soil humidity of my plants

- [part IV](/raspberry/2015/12/30/monitoring-iot-backend) - a
home-grown IOT-backend:
[limitless-garden](https://github.com/abarbanell/limitless-garden)

- [part V](/raspberry/2016/01/09/arduino-nginx): how to secure the traffic between your microcontrollers
and the backend API, before I will in the next parts go into more details

This is part three. And in a way, it is not. Because here I change
the subject from "Monitoring your Raspberry Pi" to "Monitoring
**WITH** your Raspberry Pi" - I now want to measure things from the
real world around me.

The first part already had the CPU temperature as a proxy for
environment measurements, and I could see that with open windows
in my living room the temperature was dropping with every summer
thunderstorm. But of course more can be done.

If you want to follow along, be prepared to wire up some electronics
parts. No soldering is required. Just plug an pray...

<img src="/assets/img/2015/08/sensor-photo.jpg" 
alt="Sensor Circuit on Breadboard" style="width: 100%;" />


# Starting Considerations

Before we start, I wanted to share my thoughts about some of the
choices to be made along the way, and why I decided on one approach
or another. This may not all apply to your situation, as my decisions
were influenced not only by the "best" solution, but also by parts
I had available, knowledge already in my brain, etc.

So, to start with, what do I want to measure? In my case, I want
to eventually build a system which can water my indoor and outdoor
plants during my vacation. So I will also have to control
pumps, valves etc. based on the measurements taken. But this comes
much later, first lets measure the following:

- air temperature
- air humidity
- soil humidity

There are some sensors available for this, I have chosen the 

- [DHT22](https://www.adafruit.com/products/385) for air temperature
and humidity. Reason: cheap, simple, available libraries to read
out the digital data pin.
- Soil hygrometer FC-28, available from many places, like 
[Aliexpress](http://www.aliexpress.com/wholesale?SearchText=fc-28&CatId=0&catId=),
[Amazon](http://www.amazon.com/FC-28-B-Humidity-Detection-Sensor-Module/dp/B00KV38ZZS),
 it is in pretty common use, widely available, analog output.

Hey, wait, analog? The Raspberry Pi does not have any analog IO. I
could add an anaog IO board like
[Custard-3](http://www.sf-innovations.co.uk/custard-pi-3.html) to
the Raspberry, but that looks like quite a bit of stuff and it's
not cheap, would it not be simpler just to use an
[Arduino](https://www.arduino.cc/) instead of the
Raspberry?

Clearly this would be nice for the analog I/O, but then the Arduino
is usually missing an internet connection and when it has network
it also requires add-on boards and is not that cheap any more.

There is hope in the near future, if this kickstarter project is
successfully delivering its promise:
[Wino-Board](http://www.wino-board.com/index.php/en/) - an arduino
with wifi for small money.

Ok, I have ordered some of these, but I do not want to wait, so here
is the approach: The Raspberry is connected via USB to
a small Arduino controller, which reads the sensor data and returns
it via the Serial Connection provided by the USB in a JSON format.
On the Raspberry there is a small Python script which runs every
minute via crontab, reads the latest data and sends it to the
monitoring server.

# Shopping time

People think that men do not like shopping. Here we have a proof of the
contrary. We will need to buy a few parts, and also some tools if
they cannot be found in the household somewhere.

- Arduino Microcontroller. There are many flavours, different in
processing speed, power and form factor. I have chosen a small
bread-board-compatible model ([Arduino Micro](https://www.arduino.cc/en/Main/arduinoBoardMicro))
with USB port. This makes the wiring
much easier (just plug it into a breadboard) and makes the power
supply and communication to the Raspberyy also very easy.
- A breadboard. "half size" would be sufficient, but I have used a
"standard size" (830 pins)
- some cables 
- one DHT 22 sensor
- one (or more ) soil hygrometer sensors
- A Raspberry with power supply and Internet - of course you have
this already, if you followed the two parts before. Should be a Raspberry-2 
if you also want to use it to run the development IDE.

You can go to an electronics shop, but most cities don't have this
any more. You can buy online in many places. Since this is just a
fun project for me, and neither delivery time nor quality is
absolutely critical, I have bought most of the stuff on amazon,
ebay, aliexpress directly from chinese suppliers. So far all of the
orders have worked well and within 14 day or less I had the correct
product (well, sometimes I ordered the wrong part, but
that's another story). The only time I got a chip with bent legs
was when I bought it in a shop and carried it home in my backpack.

As for tools, I have bought a multimeter, which is useful, but still 
not absolutely necessary for this project.

# Development tools

So how do we program an Arduino? The easiest is to download the
[Arduino IDE](https://www.arduino.cc/en/Main/Software) and hook up
your Arduino via USB to your PC or Mac. Works fine.

But then to run the software you need to unplug from your Desktop
and connect to the Raspberry. Not nice. Also, if you bought one of
the cheap [Arduino Nano clones](http://www.aliexpress.com/premium/arduino-nano.html) 
from China, they may have a different
serial chip (CH340 instead of ftdi), and you would need to download a driver from a 
chinese
website to your precious desktop. Can't you run the Arduino IDE
directly on the Raspberry?

Answer is: Yes, we can. You do not even need to download a driver.

But...

- the Raspberry is of course a bit slower.
- Raspbian is still based on Debian Wheezy, which does not allow
to run the newest version of Arduino IDE
[1.65](https://www.arduino.cc/en/Main/Software)
- the older Arduino IDE does not support so many Arduino types, and
none of the ones I wanted to use ([Arduino
Micro](https://www.arduino.cc/en/Main/arduinoBoardMicro), [Arduino
Nano](https://www.arduino.cc/en/Main/ArduinoBoardNano))

## Raspbian upgrade to Debian jessie

So we first do an upgrade of Raspbian to Debian Jessie, as described
in [this stackoverflow
discussion](http://raspberrypi.stackexchange.com/questions/27858/upgrade-to-raspbian-jessie).

Takes a few hours, and since we did the setup of monitoring for Raspberry
in the first two parts of this series, we can now see how the upgrade
goes:

<img src="/assets/img/2015/08/jessie-upgrade.png" 
alt="RPYM data during jessie upgrade" style="width: 100%;" />

You see that the upgrade gives the CPU a bit to do, takes about 2-3
hours on a Raspberry Model 2and needs quite a bit disk space (I
have a 8GB disk)


I have read that the jessie upgrade has some issues if you have
your display manager directly on the Raspberry, but I have not
tested this. For me, with ssh only, the issues after this upgrade
were:

- exactly none yet.

## Arduino IDE 1.6.5

Next I have combined the information from these sources: 

- [Raspberry Forum](https://www.raspberrypi.org/forums/viewtopic.php?f=66&t=92662)
- [How to install a package's build dependencies?](http://ask.debian.net/questions/how-to-install-a-package-s-build-dependencies)

This results in the following to install the 1.6.5 Arduine IDE on Raspberry Pi: 

```
wget https://github.com/arduino/Arduino/archive/1.6.5.tar.gz
tar xf 1.6.5.tar.gz
cd Arduino-1.6.5
git clone https://github.com/ShorTie8/Arduino_IDE
ln -s Arduino_IDE/debian debian

# build dependencies for Arduino IDE
sudo apt-get install devscripts
mk-build-deps

# the next line will create dependency warnings which we fix afterwards
sudo dpkg -i arduino-build-deps_1.6.5_all.deb

# clean up dependencies
sudo apt-get -f install

# make some space
sudo apt-get autoremove
sudo apt-get autoclean
sudo apt-get clean

dpkg-buildpackage -uc -b -tc
cd ..
dpkg -i arduino-core_1.6.5_all.deb arduino_1.6.5_all.deb
```

## Test the install 

How to test the install: In my case, I have no display connected
to the Raspberry, so I connect to it from my (Mac) desktop with
"ssh -X". Then on the Raspberry I start the IDE with the command
"arduino" and see it coming up on my screen:


<img src="/assets/img/2015/08/arduino-ide.png" 
alt="Arduino IDE on Raspberry" style="width: 50%;" />

## Sample program

There are sample programs included in the Arduino IDE, but I wanted
to make my own to show the communication from Raspberry to Arduno
and back.

You can try to clone this from my git repository
[abarbanell/arduino-echo](https://github.com/abarbanell/arduino-echo)

```
$ git clone https://github.com/abarbanell/arduino-echo.git
$ cd arduino-echo/echo
$ arduino echo.ino
```

In the Arduino IDE, select your Tools->Board and the Tools->Port 

- Arduino Micro with /dev/ttyACM0
- Arduino Nano with /dev/ttyUSB0

and try to push the code to the Arduino with the "Upload" button.

<img src="/assets/img/2015/08/arduino-echo.png" 
alt="Arduino IDE on Raspberry with Echo sketch" style="width: 50%;" />

# Connecting the sensors

For connecting the Hardware we use a breadboard and it should look like the image 
below. You can also download the [Fritzing](http://fritzing.org/home/)
 design file and the parts list from ithe 
["circuit" folder in my git
repository](https://github.com/abarbanell/arduino-sensor/tree/master/circuit).

<img src="/assets/img/2015/08/sensor-circuit.png" 
alt="Sensor Circuit on Breadboard" style="width: 100%;" />


# Programming the Arduino

For the Arduino we have now set up all the tools, so we can write
our program. In Arduino terms this is called a "sketch", it will
be wrapped into some standard boilerplate code during the compile
process, For example, the standard main() function does not need
to be written, it is just a few lines of code like this:

```
int main( void )
{
	// some initialization left out...

	setup();
	for (;;)
	{
		loop();
		if (serialEventRun) serialEventRun();
	}

	return 0;
}
```

You just provide the setup() and loop() functions. 

I have shared the code I use to read my two sensors on github in [abarbanell/arduino-sensor](https://github.com/abarbanell/arduino-sensor) but it is relatively short.

You need to install the [DHT-sensor-library](https://github.com/adafruit/DHT-sensor-library)
 to communicate with the DHT-22 sensor.  

Then you just open my code in the Arduino IDE and upload it to your Arduino, but of course after checking and potentially changing: 

- The Arduino board model (IDE -> Tools -> Boards)
- The serial port for upload (IDE -> Tools -> Posrts) 
- The DHT sensor model (in the code, default DHT-22) 
- The pins used for the sensors - these need to correspond to your
cabling in the phyiscal world (Default Digital D2 for the DHT-22 and
Analog A0 for the soil hygrometer)

# Programming the Raspberry

Now if we connect the arduino with the USB port of the Raspberry
it will be powered up and start automatically. On the Raspberry I
have just a simple Python script running once a minute from the
crontab, which will then read the data from the Arduino and send
it off to our statsd server:

```
#!/usr/bin/python

# monitoring script to send off sensor data from DHT22 sensor  to statsd.
# this should be called from crontab loke this: 
# 
# m h  dom mon dow   command
# * * * * * $HOME/github/abarbanell/rpym/mon-sensor.py
#
# this expects the statsd server listed in the /etc/hosts table like this: 
#
# 192.160.100.100 statsd
#


import statsd
import serial
import json
import os

# setup
host = os.uname()[1]
ser = serial.Serial('/dev/ttyACM0', 9600)

# get data 
msg = ser.readline()
res = json.loads(msg)
humidity = res[u"humidity"]
celsius = res[u"temperature"]
soil = res[u"soil"]

# send data
c = statsd.StatsClient('statsd', 8125, prefix=host)

c.gauge('sensor.humidity', humidity)
c.gauge('sensor.celsius', celsius)
c.gauge('sensor.soil', soil)
```

see also on github in the script
[mon-sensor.py](https://github.com/abarbanell/rpym/blob/master/mon-sensor.py).

Please check that the name of the USB device is correct, the value
/dev/ttyACM0 works for me with the Arduino Micro, while I also have
an Arduino Nano clone (from China for just $2.95!) which would be talking via
/dev/ttyUSB0.

So here is how it looks in the real world:

<img src="/assets/img/2015/08/plant-photo.jpg" 
alt="Plant with sensors" style="width: 100%;" />

# Conclusion

I hope you like this little toy project,
this is the data from the online presence of my plant: 

<img src="/assets/img/2015/08/plant-sensor-data.png" 
alt="Data from the plant sensors" style="width: 100%;" />

I will need to learn from this data when my plants needs to be watered
so that I can set the right thresholds later when it is about actually
pumping water into the pot.

Also, some thoughs about reducing the cabling (wifi would be nice
to save the network cable) and waterproof packaging of the lot would
be good to apply it in outside scenarios.

Stay tuned for more.

