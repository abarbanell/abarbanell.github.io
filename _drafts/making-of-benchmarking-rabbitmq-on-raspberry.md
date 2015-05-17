---
layout: post
category : raspberry
tagline: "Benchmarking RabbitMQ on Raspberry Pi"
tags : [raspberry, rabbitmq, benchmark]
---
{% include JB/setup %}


Recently I have written about how to [set up rabbitmq on a
Raspberry](/linux/2014/08/26/rabbitmq-on-raspberry-pi/).  Now I was
interested to get some performance data about this setup. There is
a great set of benchmarks on the high end spectrum - Google and
Pivotal show a benchmark [up to 1 million messgaes per
second](http://blog.pivotal.io/pivotal/products/rabbitmq-hits-one-million-messages-per-second-on-google-compute-engine). 

I will target the low end of the spectrum.

- First I would like to get some basic data about the throughput of
a rabbitmq on a raspberry.

- Then I would like to compare the different versions of the raspberry
(type 1 and type 2) because the multi-core CPU of the  type 2 should
have a strong influence on our benchmark

- Last I would like to compare Raspbian with Ubuntu Core - the new
type 2 raspberry is capable of running the official Ubuntu Core and
I would like to know how these two compare.

# Benchmark results

to be filled in later...

# separate Post: "Making of" Benchmarking RabbitMQ on Raspberry Pi

intro to be written...

# Step 1 - housekeeping

I will be using Raspbian on two Raspberry Pi 1 boxes, called rpi01
and rpi02, and one Model 2,  obviously named rpi03. So we do in
each case

```
rpi01$ sudo apt-get update
rpi01$ sudo apt-get upgrade
rpi01$ uname -a
```

the last command will return the following for Model 1 vs. Model 2

```
Linux rpi01 3.18.7+ #755 PREEMPT Thu Feb 12 17:14:31 GMT 2015 armv6l GNU/Linux
Linux rpi03 3.18.7-v7+ #755 SMP PREEMPT Thu Feb 12 17:20:48 GMT 2015 armv7l GNU/Linux
```

We see, the only difference is that the Model 2 has a armv71 machine
id and has SMP enabled because it is a multi-core CPU.


# Step 2 - get rabbitmq

I have extracted from the original documentation at
[http://www.rabbitmq.com/documentation.html]
(http://www.rabbitmq.com/documentation.html)
which steps were needed to get things up and running.

## install via apt 
(see
[http://www.rabbitmq.com/documentation.html]
(http://www.rabbitmq.com/documentation.html)).
  Use a text editor of your choice and add the following 
line to the file /etc/apt/sources.list (you need sudo privileges for this).


```
deb http://www.rabbitmq.com/debian/ testing main
```

execute the following commands: 

```
rpi01$ wget https://www.rabbitmq.com/rabbitmq-signing-key-public.asc
rpi01$ sudo apt-key add rabbitmq-signing-key-public.asc 

rpi01$ sudo apt-get update
rpi01$ sudo apt-get install rabbitmq-server
```

check rabbitmq is running 

```
rpi01$ sudo rabbitmqctl status
...
```

## configuration

configure user and [admin web ui](http://www.rabbitmq.com/management.html)

```
rpi01$ sudo rabbitmqctl add_user <user> <password>
rpi01$ sudo rabbitmqctl set_user_tags <user> administrator
rpi01$ sudo rabbitmqctl set_permissions -p / mq ".*" ".*" ".*"
rpi01$ sudo rabbitmq-plugins enable rabbitmq_management
```

now you should be able to see the management console with a web browser: 

![Rabbit console](/assets/img/2015/05/16/rabbit-console.png)

# Step 3 - get the benchmark tools

This is [a set of benchmark tools](http://www.rabbitmq.com/java-tools.html)
included in the [Rabbitmq Java client](http://www.rabbitmq.com/java-client.html).

So we head over to our desktop or laptop (Mac OS Yosemite in my case) and 

- install [java](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
- get [rabbitmq java client](http://www.rabbitmq.com/java-client.html)
- get [RabbitMQ Performance
Tool](https://github.com/rabbitmq/rabbitmq-perf-html) to get nice
performace graphs embeddable in this webpage (see below)
- set your environemnt variables and run a few benchmarks to get a feel. You can clone or fork this [project](https://github.com/abarbanell/rabbitbench) as a start. 







