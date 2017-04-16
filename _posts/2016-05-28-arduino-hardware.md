---
title: "Arduino IDE: developing flow for hardware libraries"
category: arduino
tags: [arduino]
---


I have a [winoBoard](http://www.wino-board.com) arduino compatible controller and one of the great things is the on-board [ESP8266](https://en.wikipedia.org/wiki/ESP8266) wifi chip. The board comes with a library to make it easier to "talk" to the wifi, but I found a few things missing, so I wanted to contribute.

Here I would like to describe the development setup for this type of contribution.

_EDIT: updated this for Arduino 1.6.10_

# Arduino IDE

## Fresh install

If you have not already done this, or if you are running an older
version of the Arduino IDE, first install the Arduino IDE as described
in
[https://www.arduino.cc/en/Main/Software](https://www.arduino.cc/en/Main/Software)
- latest version tested is 1.6.10

On my linux box I have extracted the download file and then moved
it to my preferred install location with

	$ sudo chown -R bin.bin arduino-1.6.10
	$ sudo mv arduino-1.6.10 /opt


and for convenience I have a desktop icon pointing to the IDE binary at /opt/arduino-1.6.10/arduino

# Install tools and library

We will later be working with the source code of the library, but we still need to install it in the regular way 
via the boardmanager, otherwise we will be missing the compiler and upload tools. 

If you open the Arduino IDE now, first go into the preferences and
change the sketchbook directory if necessary. 

Next, please navigate to the board manager and select the Wino Board: 

![IDE board manager dialog]({{ site.baseurl }}/assets/img/2016/06/ide-wino.png)

It is all described on the [Wino board website](http://www.wino-board.com/docs/quickstart)

# verify 

To verify you can compile and upload a sketch try the most simple of all, the empty sketch: 

![IDE board empty sketch]({{ site.baseurl }}/assets/img/2016/06/ide-empty-sketch.png) 

Or you also try the next simplest, the well known blink sketch:

```
void setup() {
  pinMode(2, OUTPUT); 
}

void loop() {
 digitalWrite(2, HIGH);   // turn the LED on (HIGH is the voltage level)
 delay(1000);             // wait for a second
 digitalWrite(2, LOW);    // turn the LED off by making the voltage LOW
 delay(1000);             // wait for a second
 }
```

# get the Wino library source code

First, have a look at the library folder in
[github/FKopp/WinoBOARD_Addin](https://github.com/FKopp/WinoBOARD_Addin).
Next, log in to github and fork this repository.

Then you need to clone this into your local PC: 

	git clone git@github.com:(your git username)/WinoBOARD_Addin.git

We need to link this to the location where the Arduino IDE expects
these files, replacing the library files just installed above.

The Arduino IDE expects the content of the hardware folder in s pecific structure, see [details here](https://github.com/arduino/Arduino/wiki/Arduino-IDE-1.5-3rd-party-Hardware-specification). We want to use a hardware folder inside of our sketchbook, so we do 

	cd ~/.arduino15/packages/wino/hardware/samd
	rm -rf 1.6.8
	ln -s (absolute path of the git clone directory) 1.6.8

_Note:_ The version 1.6.8 above is the version of the Wino library *not* the version of the Arduino IDE.

As a result you should now have all the files of your library git
clone inside the sketchbook/hardware/wino/sam folder.

# verify that everything is there

as a test I have added a few logDebug statements in my copy of the library code to see whether this would actually allow me to drill into the exectution of the library code: 

You can see samples in this [github commit](https://github.com/abarbanell/WinoBOARD_Addin/commit/0fea576db0156eaca86affb3336f2793f704e11d).


# Working with the code

Please refer to
[https://gist.github.com/Chaser324/ce0505fbed06b947d962](https://gist.github.com/Chaser324/ce0505fbed06b947d962)
for a detailed explanation about how to work with a forked repsitory
and create pull requests to include back in the original repo.

I also found it useful to read this blog post about [updating a
fork directly from
GitHub](http://www.hpique.com/2013/09/updating-a-fork-directly-from-github/)
in case the master has some chnages which you want to pull into
your forked repo.

# Conclusion

If everything works then you can contribute and create a pull
request like this:
[https://github.com/FKopp/WinoBOARD_Addin/pull/1](https://github.com/FKopp/WinoBOARD_Addin/pull/1)
- it got accepted and will now be part of the next version of the
library.

Let me know in the comments if you have any thoughts or questions. Improvements are always welcome.

