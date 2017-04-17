---
title: "Scaling Scrum - SAFe or LeSS, or none of them?"
category: scrum
tags: [scrum, agile]
date: 2017-04-15
header:
  teaser: 2017/04/sprint-plan.png
---

Scaling Scrum is a big discussion point in the community of Agilists. Here are a few experiences to consider when scaling SCRUM to multiple teams. Most people see this in the light of choosing one of the dominant scaling Frameworks, [SAFe](http://www.scaledagile.com/) or [LeSS](https://less.works/) - But I found enough things in each of them I did not like, so I would suggest [SYoW - Scaling Your own Way](.). 

These are purely personal views, of course. But I am very much interested in your views as well, positive or negative. Let me know in the comments below.

## What I don't like about SAFe

I just don't like the complexity of this model, and first of all the approach of having all the sprints of a Release Train synchronized with a joint Demo. I have not seen this as a practical option: 

- you need a very large room for this event
- on demo day, all participants are super-busy with "their" demo and have no time to join others
- the whole demo session extends too much.

Only meeting lovers should go for SAFe. Just kidding. If your business or your system architecture forces you to integrate all teams for the release, then SAFe probably has a point. However, you should step back and think whether these are actually impediments for a true agile team. Examples might be: 

- Release of a group of teams has to be synchronized for commercial reasons: regulations / approvals in the medical, automotive or aerospace industry, payable release upgrades for dektop software
- You have a monolithical architecture which can only be released together
- Acceptance test and/or deployment is manual, costly, time consuming. The cost of releasing prohibits a high release frequency.

Fix whatever is in your power to fix, as these impediments will hold you back from faster time to market.

## What I don't like about LeSS

The same gripe about the fixed synchronization of team sprints applies here as well. 

Additionally, the concept of feature teams which should be able to work on any code base required for their allocated feature is just reeking of chaos. No ownership for the code will result in low maintainability, slow progress due to unknown code. It is neither efficient nor enjoyable for the teams. Engineering teams take pride in well written code, and want to own whatever comes out of it in production.

Feature teams create a framework for organized irresponsability.

## SYoW - Scaling Your own Way 

We evolved our scaled Scrum in several iterations, and I could see critical breakpoints during our growth which required to adjust the process: 

- a hand full of teams in one location, this is kind of easy (we skipped this step, actually) 
- having the development teams offshore raises the complexity one level.
- having groups of teams in 2-3 locations, again one more level of complexity

Lets look at the evolution in sprints of our process development: 

## Scaling SCRUM - sprint 0: few teams in one location

We skipped this step, so I will not go into details about this. You probably just want to add a regular Scrum of Scrum session to ccordinate the teams.

## Scaling SCRUM - sprint 1: few teams in an offshore location

Initially, we started with 3-4 teams in one offshore location, Agile experience had to be built up in the team, as we were building up our processes. Furthermore, the product owners and engineering management was in a different timezone from the team.

Here is what worked for us: 

- set up a PO proxy at the offshore site. We used a small group of Business analysts which could help the PO to detail out requirements and stories, but mostly were helpful in answering questions by the development team and thus reducing the need for communication bandwidth between the sites
- talk daily, on all channels, as if you were in one room. 
- Travel frequently.

These are points which did not work good enough: 

- we kept the sprints synchronized, with all demos and planning sessions together. I would argue that this only worked because we had no other choice at the time due to our communication bandwidth. We kind of ignored the team structure and managed it from our headquarter like one big team.  Results were frustratingly long demo sessions, long planning session, even longer acceptance tests.
- Overall, we focused fully on time to market, at the expense of maintainability and test / deployment automation. I still think this was the right decision businesswise, but it created some pain.

## Sprint 1 Retrospective

One of the findings was that communication could be improved. 
We completed our magical communication square, to have two independant channels, a product channel and an engineering channel. Both were connected at each site to make sure any miscommunication could be compensated by information through the other channel.

![offshore communication]({{ site.baseurl }}/images/2017/04/offshore-communication-square.png)

You can see this as the cost of lacking colocation. 

## Scaling Scrum - sprint 2 - groups of teams in multiple locations

When we grew our organisation size, we started to correct some of the shortcuts: we built the next 3-4 teams in a different location, closer in timezone. We located PO's with the team. Most of all, we staggered the sprints so that every team was doing their demo on a different day. This allowed people to join more than just their own team's demo, especially for PO's and Srum Masters this was important. Each demo was smaller in scope, and thus shorter.

![Sprint Plan]({{ site.baseurl }}/images/2017/04/sprint-plan.png)

We selected the subsystems for each location so that the communication between locations could be reduced by using documented API's.

In each location, the teams had their demos at different days. This allowed participation of people in more than just their "own" demo.

## Sprint 2 Retrospective

We see here a good example of [Conway's Law](https://en.wikipedia.org/wiki/Conway%27s_law) - the organisation's communication structure (staggered sprints as opposed to synchronized sprints in SAFe) works well in a Microservice Architecture, but not if multiple teams work together on one monolithic application. That was also the reason why the architecture which was ok with 3-4 teams, did not hold with 8-10 teams. 

It follows from this that SAFe may be suitable (or necessary) if your system is architected as a monolith for each Agile Release Train.

Side comment - I also dislike the SAFe notion of a Release Train where all teams sit "locked in a train" and travel together from station to station. Sounds very inflexible to me. For us, it was the opposite: the release train is the release, not the sprint. If your team's sprint arrives late at the departure time, the train leaves without, and you just take the next one. With a high enough train / release frequency this is not a big problem. You need only limited planning beforehand. 

## Scaling SCRUM - sprint 3 - drive for automation

With the automation of test and deployment, we could shorten our acceptance test cycle. Some teams reached almost complete automatic testing, others were still working on the progress. We tried to let each team move  towards full test and deployment automation, but still kept a manual trigger button on the production deploy until enough teams could get to automated testing. 

Also, the PO's needed to build their confidence by experiencing that their contribution to acceptance testing was not adding value any more - since all the value had already been added earlier in the sprint, and hence the test scenarios been automated. 

It was an important decision to decouple the speed of the different teams to build their test and deployment pipeline as fast as possible, but we also dedicated time of one team to accelerate tools and knowledge sharing with the other teams. 

## Conclusion

When scaling SCRUM, look out for the communication bottlenecks in your organisation. Work can only flow if it is not held up by blockages. Iterate frequently on the product, but also on the process, to adjust your setup to your environemnt. Invest in your "last mile", how to get software from source code checkin to the production system: Continous Integration, test automation, zero-click deployments from source code repository. Reducind the cost or releasing is the key to accelerating the whole delivery.

I am very much interested in your views, please let me know what you think in the comments below. 

#### Looking for something else? 
Enjoy some of my [cat videos](https://www.youtube.com/watch?v=YPZPXDizUkU&list=PLyu5cHg7bWPjyymUCRJcpN_-fyoZzvlWh).

#### Think cat videos are bullshit? 
Then try this: [Cowshit is the new Bullshit](https://www.youtube.com/watch?v=bLTNhu8izu0).