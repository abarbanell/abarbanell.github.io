---
title: "Benchmarking RabbitMQ on Raspberry Pi"
category : raspberry
tagline: "Benchmarking RabbitMQ on Raspberry Pi"
tags : [raspberry, rabbitmq, benchmark]
---

A while ago I have written about how to [set up rabbitmq on a
Raspberry](/raspberry/2014/08/26/rabbitmq-on-raspberry-pi/).  Now I was
interested to get some performance data about this setup. There is
a great set of benchmarks on the high end spectrum - Google and
Pivotal show a benchmark [at 1 million messages per
second](http://blog.pivotal.io/pivotal/products/rabbitmq-hits-one-million-messages-per-second-on-google-compute-engine).
And there is a set of [older benchmark results from 2012]
(https://www.rabbitmq.com/blog/2012/04/17/rabbitmq-performance-measurements-part-1/) with corresponding toolset
from the makers of rabbitmq as well.

I have re-run part of these benchmarks on both generations or Raspberry Pi, the Raspberry 1 and Raspberry 2.  Just to see how a 35$ computer compares.

# Benchmark results

The first set of tests is a simple publish/consume cycle with one
publisher and one publisher in parallel. Here is how it looks on
the old armv61 CPU of the Raspberry 1:

![Summary 1->1 armv61](/assets/img/2015/05/16/summary-1-1-armv61.png)

More than 700 per sec, I am already impressed. Based on my 
[work earlier](/raspberry/2014/08/26/rabbitmq-on-raspberry-pi/)
 I was not expecting such a high throughput from a single-core CPU armv61.

![Consume 1->1 armv61](/assets/img/2015/05/16/consume-1-1-armv61.png)

compare this with the same test run on the arm71 CPU of the Raspberry Model 2: 

![Summary 1->1 armv71](/assets/img/2015/05/16/summary-1-1-armv71.png)

Almost 4x the throughput of the smaller brother. This is going to
be fun. I need to start looking for use cases which require this
throughput at a 35$ budget.

![Consume 1->1 armv71](/assets/img/2015/05/16/consume-1-1-armv71.png)

In both cases you can see that the publisher by far outpaces the
consumer and gets throttled when the queue gets longer. This
results also in the consumer still running for 10-20 sec after the
publisher finished. Accordingly the latency of the messages is given
by the queuing in the rabbitmq. With a slower publisher the latency
would go down significantly - this could be shown in a future test
where we ramp up the publisher speed to show the impact on latency.

But the total impact of this effect is vastly different: 

- the old armv61 consumes a surprisingly good 700 messages per
second, and the publisher has to wait every second or so.
- but on the new armv71 thanks to 4 CPU cores the output is much
higher and allows more regular input.
- as a result the old armv61 needs 20 sec to drain the queue at the
end of the test (and the latency of the messages accordingly is at
20 sec) while the multi.core armv71 only needs 10 sec to drain the
queue.

# Ramping up

One observation from the initial test was that the producer gets
throttled by the consumer. Adding a second consumer process should
help? Here are the results:

![Summary 1->2 armv61](/assets/img/2015/05/16/summary-1-2-armv61.png)
![Summary 1->2 armv71](/assets/img/2015/05/16/summary-1-2-armv71.png)

Absolutely neglegible improvement, Lets look at the details: 

![Consume 1->2 armv61](/assets/img/2015/05/16/consume-1-2-armv61.png)
![Consume 1->2 armv71](/assets/img/2015/05/16/consume-1-2-armv71.png)

Actually, the latency on the old Raspberry gets even longer, and
the producer can only send in short, sharp bursts. 

# Summary

When stretching to the limit then something is going to be near
breaking point - in this case it is the latency induced by the
queuing. The publisher only gets throttled when a certain queue
length s reached.

Overall still absolutely impressive numbers and the system keeps
up with the traffic by throttling the publisher so that the system
internally can keep delivering.

# What's next?

I plan to elaborate in a next post how the setup was done for this
test and could also dive deeper into different test scenarios to
isolate the bottlenecks of the Raspberry (slow CPU, slow disk, slow
Network) and expose then in separate tests.








