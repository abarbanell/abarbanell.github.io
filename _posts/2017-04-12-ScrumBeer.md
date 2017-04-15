---
title: "ScrumBeer Lausanne"
category: scrum
tags: [scrum, agile]
date: 2017-04-12
published: false
header:
  teaser: 2017/04/offshore-communication-square.png
---

Todo: update teaser image
give overview of talking points today
tweet about it.

when ready -> set published flag to true, or remove.

topics: 

- less framework is now the official scaling framework for  scrum alliance (see global scrum gathering San Diego) (this is good for a few tweets and retweets) 
- remote teams / remote companies: not easy to mix colocated and remote workers in one team, but both extremes can work. Our experience: colocate teams, where possible withe the PO. Look at communication flows from business stakeholders to the team. Often, domain knowledge is more critical than technology: Team can work n technology on its own, but needs external communication for domain knowledge. In many cases there is only a limited number of domain experts available for the teams. Precious resource.
- product owner role: in Olivier's company the PO has all the budget responsibility, and becomes kind of a line manager for the team. There is currently no Scrum Master. odd. Argue a case that PO and SM should be different from line management and there is a place for a line manager in an agile company.
- Safe vs. Less. my rant. What I did not like in each of them. Then what I do like: Inspect and adapt. Know your situation and act accordingly. We built a system with 10-12 teams - each demo on a different days - allowed PO's, SM's and others to join demo's of related teams outside of their rush days (demo, retro, planning). Delivery pipeline adapted to each subsystems capability on the spectrum from manual UAT testing to continous delivery. Work with each team to move their capability at their own pace (but set them goals, manual testing is a kind of technical debt)

## Scaling SCRUM - sprint 1

Here is a brief overview of my experiences with scaling scrum, what worked for us and what did not. 

Initially, we started with 3-4 teams in one location, Agile experience was low, and we were building up our processes. Furthermore, the development team was offshore and the product owners in a different timezone from the team.

Here is what worked for us: 

- set up a PO proxy at the offshore site. We used a small group of Business analysts which could help the PO to detail out requirements and stories, but mostly were helpful in answering questions by the development team and thus reducing the need for communication bandwidth between the sites
- talk daily, on all channels, as if you were in one room. Travel frequently.

These are points which did not work good enough: 

- we kept the sprints synchronized, with all demos and planning sessions together. I would argue that this only worked because we had no other choice at the time due to our communication bandwidth. We kind of ignored the team structure and managed it from our headquarter like one big team.  Result was frustratingly long demo sessions, long planning session, even longer acceptance tests.
- Overall, we focused fully on time to market, at the expense of maintainability and test automation. I still think this was the right decision businesswise, but it was a pain.

## Scaling Scrum - sprint 2

When we grew our organisation size, we started to correct some of the shortcuts: we build the next 3-4 teams in a different location, closer in timezone. We located PO's with the team. Most of all, we staggered the sprints so that every team was doing their dempo on a different day. This allowed people to join more than just their own team's demo, especially for PO's and Srum Masters this was important.

we completed our magical diamond for the offshore communication, to have two independant channels, a product channel and an engineering channel. Both were connected at each site to make sure any miscommunication could be compensated by information through the other channel. 

![offshore communication]({{ site.baseurl }}/images/2017/offshore-communication-square.png)

You can see this as the cost of lacking colocation.