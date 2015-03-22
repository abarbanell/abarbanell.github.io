---
layout: post
category : raspberry
tagline: "Benchmarking RabbitMQ on the Raspberry"
tags : [raspberry, rabbitmq, benchmark]
---
{% include JB/setup %}


Recently I have written about how to set up rabbitmq on a Raspberry.
Now I was interested to get some performance data about this setup.

First I would like to get some basic data about the throughput of
a rabbitmq on a raspberry.

Then I would like to compare the different versions of the raspberry
(type 1 and type 2) because the multi-core CPU of the  type 2 should
have a strong influence on our benchmark

Last I would like to compare Raspbian with Ubuntu Core - the new
type 2 raspberry is capable of running the official Ubuntu Core and
I would like to know how these two compare.

# Step 1 - wait

wait for the Raspberry type 2 to be delivered so that we can actually
start....

In the meantime we can set up our benchmark and run on the old type
1 Raspberry with raspbian.


