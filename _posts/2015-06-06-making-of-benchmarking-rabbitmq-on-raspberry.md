---
title: "Making of: Benchmarking RabbitMQ on Raspberry Pi"
category : raspberry
tagline: "Benchmarking RabbitMQ on Raspberry Pi"
tags : [raspberry, rabbitmq, benchmark]
---

In my last post I have shared some results from [Benchmarking RabbitMQ on Raspberry Pi](/raspberry/2015/05/17/benchmarking-rabbitmq-on-raspberry/).  

I have also promised to go into more detail about the setup for these benchmarks later. So here we go:

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
Linux rpi01 3.18.11+ #781 PREEMPT Tue Apr 21 18:02:18 BST 2015 armv6l GNU/Linux
Linux rpi03 3.18.11-v7+ #781 SMP PREEMPT Tue Apr 21 18:07:59 BST 2015 armv7l GNU/Linux
```

We see, the only difference is that the Model 2 has a armv71 machine
id and has SMP enabled because it is a multi-core CPU. The Kernel version also shows a '-v7' suffix, but otherwise same number.


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
```

In my benchmarks I have used RabbitMQ version 3.5.3

## Step 3 - configuration

configure user and [admin web ui](http://www.rabbitmq.com/management.html)

```
rpi01$ sudo rabbitmqctl add_user <user> <password>
rpi01$ sudo rabbitmqctl set_user_tags <user> administrator
rpi01$ sudo rabbitmqctl set_permissions -p / mq ".*" ".*" ".*"
rpi01$ sudo rabbitmq-plugins enable rabbitmq_management
```

now you should be able to see the management console with a web browser: 

![Rabbit console](/assets/img/2015/05/16/rabbit-console.png)

# Step 4 - get the benchmark tools

This is [a set of benchmark tools](http://www.rabbitmq.com/java-tools.html)
included in the [Rabbitmq Java client](http://www.rabbitmq.com/java-client.html).

I have shrinkwrapped the installation of these tools and of [rabbitmq/rabbitmq-perf-html](https://github.com/rabbitmq/rabbitmq-perf-html) on github [abarbanell/rabbitbench](https://github.com/abarbanell/rabbitbench) so you can just fork or clone this as a start. 

## HOW TO RUN

first, make sure you have java installed, then clone this [repo](https://github.com/abarbanell/rabbitbench) and from within the repo folder, do: 

```
$ ./setup.sh
```
to set up the tools, then you will need to edit the file 'spec.js' in each of benchmark scenario folders './scenario-*'
 to have valid AMQP URI's for your rabbitMQ broker. You can also create new scenario's by copying  the 'spec.js' file to a new scenario folder.

Then run your scenario's with 

````
$ ./bench.sh
```

This will create (or overwrite) the result.js in each of the scenario folders.

You can visualize this with the report.html file in each scenario file. Depending on your browser you may not be able to view this via the "file://.." url, then you just could just fire up a local web server like so: 

```
$ python -m SimpleHTTPServer
```

# Conclusion

The results of 
this exercise have been already explained in my last post 
[Benchmarking RabbitMQ on Raspberry Pi](/raspberry/2015/05/17/benchmarking-rabbitmq-on-raspberry/)
so I recommend you head over to see what I got out of it.


