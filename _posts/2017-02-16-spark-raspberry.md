---
title: Apache Spark on a Raspberry Pi
category: scala
tags: [scala, sbt, vscode]
---

Valentine's Day has just passed and I still have some Raspberry Pi
left, so I thought I try something out. 

Can we run an Apache Spark on a Raspberry Pi? 

# Yes we can

I have taken a very small program which listens to a twitter feed, 
filtered by a set of predefined words and 
outputs the most frequent hashtags (every 15 sec) and the latest tweets
(every minute). It is basically one of the examples in Andy Petrella's 
[spark-notebook](http://spark-notebook.io) converted into a 
standalone spark application. It does not actualy use a Hadoop cluster, 
only the spark libraries locally.

The sources are in my repo 
[abarbanell/tweetfeed](https://github.com/abarbanell/tweetfeed) and it is simple 
to use.

You need a  Raspberry Pi with Java 8 and sbt, if you follow my blog you have 
already seen this part in a previous post about 
[Scala on Raspberry - updated](http://blog.abarbanell.de/linux/2017/01/21/scala-rpi/) 

Next you clone my repo with 

```
$ git clone https://github.com/abarbanell/tweetfeed.git
$ cd tweetfeed
```

You will need to register on http://dev.twitter.com for some API keys: register as a 
developer, create your application, and create API tokens for it, you need the following
values in your file ```.conf.rc```: 

```
# this file should be gitignore'd
 
export TWITTER_CONSUMER_KEY="YOURKEY"
export TWITTER_CONSUMER_SECRET="YOURKEY"
export TWITTER_ACCESS_TOKEN="YOURKEY"
export TWITTER_ACCESS_TOKEN_SECRET="YOURKEY"
``` 

Next is to run the tweetfeed app with 

```
$ . .conf.rc
$ sbt run -mem 512 
```

this ``` -mem 512 ``` restricts the application to 512 MB memory, which works 
on the Raspberry Pi 3 which has total 1 GB memory. With less memory the spark 
libaries won't start.

Be prepared to wait a bit for all scala and spark libraries downloaded by the 
sbt tool. You will probably need 5-10 min to start the first time, and later it 
should be around 1 min to start.

It will beautifully list the recent tweets with the given filter words and, while 
not using much CPU, the memory footprint is of course significant.

![rpym dashboard]({{ site.baseurl }}/assets/img/2017/tweetfeed-statsd.png)

The program in my repo (as of now) will do this for 5 minutes and then 
quit. Of course it is easy to change, and I may change this in the future. 
Just read the code to check if you are interested.

# Why bother? 

Just because we can. Of course we can now build more beautiful output and more 
fancy data algorithms, I just wanted to show that it can work.

So if you still have some Rasperry Pie, errh Pi left over, give it a go.

# Conclusion

I hope this was helpful, you may also like the articles on my blog about 
[my spark notebooks](http://blog.abarbanell.de/mysnb) or, for less technical 
fun, how about some [romantic valentines day videos](https://goo.gl/qT7GBy)?

You are always welcome to comment below.
