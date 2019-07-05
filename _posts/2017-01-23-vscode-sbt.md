---
title: SBT task configuration for VS Code
category: scala
tags: [scala, sbt, vscode]
---

Here is my tasks.json for using Visual Studio Code 
with sbt projects: 

<script 
    src="https://gist.github.com/abarbanell/893b247a35dd2ac4a2de9a70acf9f2f0.js">
</script>

# Why bother? 

There are two main IDE's used with scala, Eclipse and IntelliJ. But for my taste 
both of them are too heavy and resource hungry. 

I am used to Visual Studio Code because it is excellent for Javascript and Typescript, 
so my node.js and Angular projects work very well. I also do a fair amount of work on 
small cloud servers and devices like Raspberry Pi, so I want to build each project from
command line by pulling the project from github, executing a build script and running the 
project.

This means the build scripts must be able to run without an IDE. So I settled on sbt for 
my Scala projects which can even be run on a Raspberry Pi with some 
[memory tweaks](http://blog.abarbanell.de/linux/2017/01/21/scala-rpi/).

This gives two options: 

- running the build commands from command line completely outside the IDE
- or integrating the build commands.

I want to have both options at my fingertips, so I wanted to configure the following sbt 
commands in Visual Studio Code: 

- sbt run
- sbt test
- optional: sbt compile and/or sbt package 

Visual Code has a configuration for external tasks in the file .vscode/tasks.json in 
your project, and the file is described on the 
[VS Code Website](https://code.visualstudio.com/Docs/editor/tasks)

You can configure several tasks for one main command. The task with the name "build" also 
has a keyboard shortcut out of the box.

The configuration shown above is the one which works for me as a starting point. Maybe I 
will add or change something later, but for now it does the job.

# Conclusion

I hope this may be of help, at least this note will serve as a reminder 
to myself how to configure sbt tasks in Visual Studio Code.

Meanwhile, feel free to let me know what you think in the comments below, 
or enjoy some of my 
[cat videos](https://www.youtube.com/watch?v=YPZPXDizUkU&list=PLyu5cHg7bWPjyymUCRJcpN_-fyoZzvlWh)

Think cat videos are bullshit? Then try this: 
[Cowshit is the new Bullshit](https://www.youtube.com/watch?v=bLTNhu8izu0)







