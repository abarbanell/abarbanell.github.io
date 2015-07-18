---
layout: post
title: "RabbitMQ on Raspberry Pi"
description: ""
category: raspberry 
tags: [raspberry, rabbitmq, linux]
---
{% include JB/setup %}

Playing around with [RabbitMQ](http://www.rabbitmq.com) lately, 
I was interested how it would perform on a very small box, a 
[Raspberry Pi](http://www.raspberrypi.org/).

## HowTo

Here is a short description how to get RabbitMQ running on a Raspberry Pi.

1. get a Raspberry Pi installed with Raspbian – just follow the standard installation 
  guide from [raspberrypi.org](http://www.raspberrypi.org/downloads/)
1. get the rabbit MQ package from [rabbitmq.org](http://www.rabbitmq.com/install-debian.html)
1. I wanted to run a client in Node.js, so I got the latest node with these commands:

```
wget http://node-arm.herokuapp.com/node_latest_armhf.deb
sudo dpkg -i node_latest_armhf.deb
node -v
```

1. pick the node version of the RabbitMQ tutorials from 
  [https://github.com/squaremo/amqp.node/tree/master/examples/tutorials]
  (https://github.com/squaremo/amqp.node/tree/master/examples/tutorials)

## Pictures

Here is how this looks in pictures:

[![RaspberryPi running RabbitMQ]
({{ site.url }}/assets/img/2014/08/612px-6eaee38f5075337736563784792ae91a.jpeg)]
({{ site.url }}/assets/img/2014/08/6eaee38f5075337736563784792ae91a.jpeg)

[![node.js client running with rabbitMQ on a Raspberry Pi]
({{ site.url }}/assets/img/2014/08/612px-090b224d539ec598df2df725973bf488.jpeg)]
({{ site.url }}/assets/img/2014/08/090b224d539ec598df2df725973bf488.jpeg)

## Conclusion

And the results: If you let a publisher with a simple “hello world”
message run in a loop for 1000 times and start a subscriber for this
queue at the same time, you will see the 1000 messages flying through your
little Rabbit Pi in about 13 seconds, more than 50 messages per second!

You will also notice that there is hardly any concurrency between the
publisher and subscriber, so the messages only get consumed when the
publisher gets idle. This is contrary to what I see on a desktop linux
system where the subscriber always “wins” and keeps the queue empty,
and the publisher is slowed down to the pace of the subscriber.

You probably do not want to push the little Raspberry to a message rate
of many messages per second, but it is nice to know that you could!


