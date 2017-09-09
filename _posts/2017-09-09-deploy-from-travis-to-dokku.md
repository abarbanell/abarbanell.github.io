---
title: "Deploying from Travis CI to dokku"
category: linux
tags: [cd, agile, linux, devops]
date: 2017-09-09
header:
  teaser: 2017/09/travis.png
---


Welcome to world of continous deployment...

Here I will describe how to set up a continous deployment step from Travis CI to dokku for a node.js app.

![Travis Build history]({{ site.baseurl }}/assets/img/2017/09/travis.png)

# Intro

I have a node.js app called [Limitless Garden](https://github.com/abarbanell/limitless-garden) which I run on a [dokku](http://dokku.viewdocs.io/dokku/) server hosted at [digitalocean](https://www.digitalocean.com/).

Dokku is an open source project which understands the same buildpacks as [heroku](http://heroku.com) - so it allowed me to have "my own heroku" for a small price of running a 1GB server for 10 US$ per month. 

A while ago I have already integrated [Travis CI](https://travis-ci.org/) with my github repository and now every commit is tested automatically. 

After successful CI tests I used to manually deploy from my laptop with 

```
git push dokku master
````

but after forgetting to do this a few times and consequently searching in production why my new changes did not work, I remembered the old knowledge that automation is superior to human unreliability. So I needed travis to deploy to dokku on successful test.

# Prerequisite

- node.js app for dokku -> example [limitless-garden](https://github.com/abarbanell/limitless-garden)
- [dokku](http://dokku.viewdocs.io/dokku/) server hosted on [digitalOcean](https://www.digitalocean.com/) - they even have a ready configured [droplet](https://www.digitalocean.com/products/one-click-apps/dokku/) for this.
- [Travis CI](https://travis-ci.org/abarbanell/limitless-garden/builds) connected CI

# Let's go.

There are in principle quite good descritiptions of the parts needed here: 
- [dokku setup](https://github.com/dokku/dokku/blob/master/docs/deployment/user-management.md)
- [travis](https://docs.travis-ci.com/user/deployment/custom/#Git)

But some small details did not quite work for me so here is a slightly extended remix of these steps.

You need to have an ssh key for deployment to dokku, with the private key in your travis VM, and the public key in your dokku server. You do NOT want the private key readable in your github repo.

## Step 1: create a ssh key which allows deployment to dokku

Create a key pair somewhere in a safe place - you do not want the private key to leave your system.

```
ssh-keygen -t rsa -b 4096 -f deploy.key
````

copy the content of the public key file deploy.key.pub on your dokku server into the authorized_keys file of the user dokku: 

```
ssh dokku@<dokkuserver>
vi .ssh/authorized_keys
# insert content of your public key file
````

## Step 2: adjust gitignore 

You want to make sure that only the encrypted private key is in your github repo. So the unencrypted version should be .gitignore'd

I use the convention of naming the key pair xxx.key and xxx.key.pub so my .gitignore contains: 

```
# Credentials
.travis/*.key
.travis/*.key.pub
```


## Step 3: encrypt the private key and put it in your source code repo

get the travis CLI to make the process easier

``` 
gem install travis
```

copy the deploy.key into your git repo into the .travis folder: 

```
cd myrepo
mkdir .travis
cp <pathtomydeploykey>/deploy.key .travis
```

encrypt the key

```
cd myrepo/.travis
travis login
travis encrypt-file deploy.key --add
```

This command does three things: 
- encrypts the file deploy.key into the file deploy.key.enc
- adds a section "before_install" to your .travis.yml to decrypt the file in your running travis VM
- adds two variables to your travis environment with the decrytion keys. 

## Step 4: add after_success section to travis.yml for the deployment

This is an extended version of the documentation at the travis website, I had to add commands to add the dokku server to the known_hosts and also to set the push.default configuration for git to squelch a warning message: 

```
after_success:
  - eval "$(ssh-agent -s)" #start the ssh agent
  - chmod 600 .travis/deploy.key # this key should have push access
  - ssh-add .travis/deploy.key
  - ssh-keyscan <your dokku host> >> ~/.ssh/known_hosts
  - git remote add deploy <your dookku git uri>
  - git config --global push.default simple
  - git push deploy master
```

## build and deploy

Now whenever I commit something to my git repo and push to the github server, and the travis CI is triggered, it will also decrypt my key and deploy to dokku via "git push"

# Conclusion

This should help me streamlining my development workflow. I hope it is useful for you, please do let me know in the comments below.

In any case you may also like my [cat videos](https://www.youtube.com/playlist?list=PLyu5cHg7bWPjyymUCRJcpN_-fyoZzvlWh).




