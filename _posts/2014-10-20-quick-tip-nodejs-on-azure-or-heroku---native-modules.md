---
layout: post
title: "Quick tip: node.js on Azure or Heroku - native modules"
description: "Why I moved back to heroku"
category: linux
tags: [heroku, azure, node.js]
---
{% include JB/setup %}

# Quick tip: Node.js on Azure or Heroku - native modules

This is a quick tip why I moved one of my websites back from Azure
to Heroku.

On both platforms I was happy with the ease of deployment from a git
repository to the hosted site. The problem came when one of my node
modules included a native module.

## The problem: native Node.js modules on Azure

Azure can by default not compile native modules. There are workarounds
to this but they did not work for me. In the end it would be the easiest
to include the native module with all the other modules in the git repo
and let it get deployed.

Disadvantage is that this forces you to develop on Windows to get the
native module compiled in the right platform.

Apart from personal taste, there are other problems to consider then:
In many cases your node project will contain nested node_modules folders
for all the dependancies of your project. This can nest quite deep and
go beyond the limit of Filename length on Windows.

There are workarounds though, but here it got messy for me and I decided
not to solve this problem but to take an easy way out.

## The solution: Heroku

Heroku does two things different: First, it compiles all the native
modules during deployment. making sure there is no need to have the same
platform in your local development as they have for hosting.

Second, using Linux, long filenames are no problem.

They would have to do only one of these to get me up and running, but
now even better.

# Conclusion 

Back to Heroku at least for node.js projects. Azure is still nice and
will host my .NET projects.


