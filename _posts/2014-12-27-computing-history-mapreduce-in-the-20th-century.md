---
title: "Computing History: MapReduce in the 20th century"
description: "A non-fictional fairy tale of MapReduce jobs in the old times"
category: algorithms 
tags: [mapreduce, history, algorithms, google, software patent, network]
---


This is the time of the year, between Christmas and the end of the year,
where many people bring up fairy tales of their personal history, which
friends and family then silently laugh about, and on the outside at
least pretend to admire.

This story is one of them. 

<!--more-->

So here we go - this is a story about a implementation of mapreduce
algorithms before I even knew the word. In fact, it pre-dates Google's
[patent on
Mapreduce](http://patft.uspto.gov/netacgi/nph-Parser?Sect1=PTO1&Sect2=HITOFF&d=PALL&p=1&u=/netahtml/PTO/srchnum.htm&r=1&f=G&l=50&s1=7,650,331.PN.&OS=PN/7,650,331&RS=PN/7,650,331)
by more than 10 years.


So, let's go back to the year 1991. A young, hopeful physics graduate
(me) had a computing problem as part of his thesis, but the computing
powers at his disposal where limited. There was a desktop computer on
the desk in his lab -
not a PC, those where not commonplace those days, it was a [Atari ST
](http://en.wikipedia.org/wiki/Atari_ST) - also it was not a very
powerful machine,
and already quite outdated - but it had one feature I still miss in
today's computers: NO fan, NO moving parts, NO noise emission.

But I digress.

Some people had access to a mainframe computer with more horsepower,
but there was an eternal fight for computing time and just getting on
the list for it was a chance I could not take for this project. After
all it was very important for me, but not necessarily for the guardians
of the mainframe, who were in a different university department.

So, I had a desktop only. In fact, every student in the lab had one. And
they only used them during day time. So I had a lot (by the standards
of those days) of unused compute power, there had to be a way to use that.

## The application problem

But first a few words about the nature of my computing
challenge. I had to build a simulation for the effects on cosmic
radiation on material on board the very first space shuttle
missions. I am talking about the [NASA Long Duration Exposure
Facility](http://en.wikipedia.org/wiki/Long_Duration_Exposure_Facility)
- shortnamed LDEF.

It was basically a "dumb" tube the size of a bus, or more precisely the
size of the spaceshuttle loading bay,
with a lot of different experiments fixed on the tube. Our universtiy
had a stack of plastic film screwed on and this was exposed to the space
environemnet for years and then, after circling around the earth 32
thousand times, was captured and brought back to earth. This was from
April 1984 to January 1990.

My task was to create a simulation of the impact of cosmic radiation on
those pieces of plastic, and then compare to the real measurements.
So, for each segment of that trajectory, there was a calculation of cosmic 
radiation creating some damage on the material, taking into account not 
only the position, but also the time in the solar cycle, so there were as 
many points to calculate as could be done in a reasonable compute time.

Each of the points could be calculated independantly on a few parameters
and some static data, and only needed to be added up later. The
calculation was completely CPU-bound.

## Architecture - Loosely coupled MPC

The perfect choice for this problem was a [loosely coupled multiprocessing
system](http://en.wikipedia.org/wiki/Multiprocessing#Loosely_coupled_multiprocessor_system)
- the calculations were completely independant, and there was only a
very small amount of input and output data to distribute and collect.

I had to build one of those.

Having identified the compute nodes on my co-students desks I found them
just a little bit too loosely coupled - in fact there was no network at
all. Actually, that was expected as normal those days.

So I had to build a network. The requirements were simple: Given the size
of the code, the input data and the output data of a nodes combined to
be about 500 Kb, a data packet would comfortably fit on the 720KB floppy
disk which was available on each computer. With the compute-bound
problem a packet rate of 1/day on the network was enough. The
common solution to this type of networking was called [Adidas
network](http://www.urbandictionary.com/define.php?term=Adidas+network)

## Map

Imaging your young student putting the code and input data on a set
of floppy disks every day, pushing one of them manually in each computer in
the evening when other students were leaving, and starting the program to run
overnight and write its data on the floppy. This was the map step,
mapping each segment on the satellites trajectory to the contribution
of cosmic radiation from this segment.


## Reduce

Then in the morning, as the map
job was finished, the data was collected, and the floppies were one by
one inserted in my desktop. On this one I had the reduce job installed,
aggregating all the data from the floppies and calculating the total
exposure of my material to cosmic rays.

## Conclusion

So were did this lead to? I got my degree, Google got their patent. we
did not know of each other. Is this fair? I bet not. I was there
before. But even I may not be the first.


