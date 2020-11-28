---
layout: post
title: JxT
description: >
  Who says those two can’t play together?
author: author1
image: /assets/img/2020-xx-xx-JxT/Rooney_Tevez.jpg
noindex: true
categories: [football]
tags: [Football, Premier League, Analytics]
---

Not exactly one from the football commentary hall-off-fame, but hardcore Man Utd fans may remember the above words from Rob Hawthorne following [Carlos Tevez’s goal to make it 3-1 against Middlesbrough in October 2007](https://youtu.be/kVuD1aElYxQ?t=85) (friendly reminder that Nani scored an absolute banger for 1-0). Tevez’s placed finish came off an intelligent backheel assist from Wayne Rooney, whose awareness of the situation to draw 3 defenders to him was matched by Tevez’s anticipation of the play. Tevez’s arrival in the summer was met with skepticism over whether the duo could form an effective partnership due to their similar all-action and selfless style. By the end of that season, questions over their chemistry were [well and truly put to bed](http://therepublikofmancunia.com/tevez-rooney-and-i-proved-everyone-wrong/) as they, along with Cristiano Ronaldo in a fluid front-three, led Man Utd to the European Double and most successful season since the millenium. 

Player Chemistry I believe, is generally accepted by the football community and fans to be a “real” thing. Heck, it even is a relatively important thing in Fifa Ultimate Team, where the term is probably most familiar to many. 

![FUT](/assets/img/2020-xx-xx-JxT/fifa-20-chemistry.jpg)

At the end of my [first proper blog post](https://glad94.github.io/football/2020-06-11-Best-Mates/), I mentioned a [paper by the team at SciSports who developed a Player Chemistry-quantifying metric](http://www.sloansportsconference.com/wp-content/uploads/2020/02/Bransen_paper_player_chemistry.pdf) using their VAEP framework (Bransen, Van Haaren, 2020). Their paper began with the example of then record signing Andy Carroll and Luis Suarez’s contrasting performances at Liverpool, supporting the notion that big money signings are doomed to fail if there’s no chemistry due to a mismatch in playing style. Perhaps more recent examples can further convince you of this “truth”, with Barcelona’s record signings Antoinne Griezzman and Philippe Coutinho performing nowhere near the levels which warranted their purchase. As mentioned in Bransen and Van Haaren, despite the importance of player chemistry, this is still a largely overlooked aspect in player evaluation for recruitment purposes, along with existing performance evaluation being largely individual-focused (I’m not sure how much this has changed since tbh). Thus, this analysis using JxT represents an attempt to add to the growing work in quantifying player chemistry in football and its uses.      

## Credit where credit’s due 
When I was reading through Bransen and Van Haaren’s derivation of their Player Chemistry metric, I was rather pleased to see the appearance of shared player minutes (which was the topic of my first post) in their formulas, which meant that I could put the numbers I painstakingly computed to further use, because why not? Hence, a large part of what I’ll describe in the following sections is really just a replication of their work, with the exception of the underlying value metric that’s being used (VAEP in their case). What’s the underlying metric being used here then? You probably could’ve guessed it from the title. That’s right, Expected Threat (xT), a term that should be general knowledge to the football analytics community. I won’t elaborate much on xT beyond saying that it is one of several models that value how actions increase or decrease the likelihood of scoring a goal and often used to quantify player performances. For readers that are new to this term, I defer you to this [marvellous write-up and explanation of the xT metric](https://karun.in/blog/expected-threat.html) by its creator Karun Singh. 

As you’ll see later, most of the heavy lifting has been done by these previous pieces of work, hence the credit really belongs to them. The name of this Player Chemistry metric, Joint Expected Threat (JxT) is also simply the merger of the ‘J’ from Bransen and Van Haaren’s Joint Offensive Impact (JOI) metric and xT. 

## Deriving JxT
Although my formulation of JxT was originally motivated by JOI, let’s try to derive this from a more fundamental angle and assume we’re starting from scratch. First, we start with the notion that mutual chemistry between players is positively correlated with their performances. From this, we can naturally assume that two players with high mutual chemistry perform better than two players with low mutual chemistry, all other things being equal. Thus, we want a performance metric that measures this joint performance between a player pair. We could use several advanced performance measuring metrics (e.g. xA, key passes) but let’s keep things simple and make use of existing models that assign single-dimensional values to on-the-ball actions anywhere on a football pitch (e.g. xT, g+, PV). Another positive is that these typically operate on event-level data, a good sample size of which is openly available today thanks to the initiatives of Wyscout, Statsbomb and others*.   

With that in mind, we thus want to formulate a metric that: 

1. Is built on an existing value-attributing base model that operates on event-level data, due to its decent availability compared to e.g. tracking data.  

2. Rewards actions/events (e.g. passes, dribbles) between any two players that contribute to their attacking chemistry. As a starting point let’s term this subset of events **“joint interaction actions”** between the pair. 

Let’s try to gain some intuition for what JxT should look like from event-level data (remember, event-level data only involves actions on the ball, so actions like off-ball runs aren’t logged). I’ll use a ridiculous goal that screams Player Chemistry to me.


Point 2 in the framework requires us to define what constitutes joint interaction actions in event data. We can first take the literal meaning of **“Joint”** and concatenate all events that originate from the two players. Taking Messi and Alves from the above example, that’s simply every event by them. Next, we need to further subset this sequence to contain **“Interaction”** events. A first logical choice would be the passes between each other. Additionally, we should also consider the immediate events that arise from the preceding exchange (e.g. 1. Messi’s pass to Xavi (after receiving Alves’s pass), 2. Messi’s dribble after receiving Alves’s pass). With event data, these requirements are satisfied by defining an interaction sequence to be consecutive events alternating between the two players. Using these event filters, we can see the events which constitute Messi (M)-Alves (A)’ joint interaction actions to then be something like:

*M pass, A pass, M pass, A pass, M pass (to Xavi), M pass, A pass, M pass, A pass, M dribble, M pass, A pass, M shoot (goal).*

With our definition of joint interaction actions settled, all that’s left is to select a base model to assign values to each action in the sequence. I’ve chosen xT simply because it’s the first action-value assigning model I came across, although the choice should be quite flexible. Since we’re using xT as the base individual player metric, JxT should also be calculated in a manner faithful to xT (the below may not necessarily apply for a different base metric).  

1. Rewards only 'successful' events (passes, dribbles) within an interaction sequence, i.e. moves that were completed without possession being lost.

2. Rewards actions independent of the end outcome of the possession (i.e. although Messi’s shot is counted as part of the interaction sequence, no reward is assigned to it).

Finally, let’s put everything together into several formulas for better clarity. 

### JxT for player comparisons

While JxT can be applied for player pairs over single matches up to a season, to enable a fair comparison between pairs who spent different amounts of time on the pitch together within a season, we introduce JxT/90 which weighs total JxT against the pair’s shared minutes: 

### Subtleties (comparison with JOI)

I mentioned at the start of the above paragraph that my formulation of JxT was originally simply a reapplication of the JOI metric by Bransen and Van Haaren on the xT metric. In some sense while I did try to backwards-engineer the JxT derivation process for the benefit of the reader’s intuition, ultimately both methods still share the same underlying principles. The main difference in xT vs VAEP application for this purpose is that JOI approach rewards VAEP for shot outcomes whereas JxT ignores them. I did experiment with a rough shot rewarding approach by using xT as a proxy for xG but I did not like how it swayed the results quite heavily for strikers. 

### I’ve seen this somewhere before... 

JOI aside, you may have come across a pretty similar concept to this before in several versions of passing networks, e.g. 

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Premier League Gameweek 4:<br><br>Liverpool (A) vs Aston Villa (2-7) <a href="https://t.co/58jd5p2MAw">pic.twitter.com/58jd5p2MAw</a></p>&mdash; Omar (@topimpacat) <a href="https://twitter.com/topimpacat/status/1313173254517985281?ref_src=twsrc%5Etfw">October 5, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
Totally coincidental choice of match result here
{:.figure}

where the edges are coloured according to their strength of the passing link between 2 players. In fact, such visualisations probably originated from Karun’s idea of interactive passing networks, which assigned directional weights to the edges based on the value from passes given/received values (he was using a version of xT to calculate the values here before he published about xT). While the ideas are indeed quite similar, the main difference is that the joint interaction action sequences don’t only include passes and dribbles between the pair of players. 

## Observations and Use Cases

With the JxT formula in place, it’s now time to put the metric to test on actual event data and have the resulting player pair JxT values to also serve as a sanity check. Following this, several possible applications for JxT in the areas of team selection/evaluation and player recruitment are presented.  

The data used for this section are as follows: 

1. Event data used encompasses 5 leagues’ worth of data from the 2017/18 season released as part of Wyscout’s open data initiative. Event-level data constitutes a list of sequential events along with basic attributes for each event, e.g. the player in possession, time elapsed, start and end locations, event outcome etc. 

2. A xT model trained on 2017/18 Premier League data provided by Karun [here](https://twitter.com/karun1710/status/1156196523765633024) is used. The original 12x8 grid is then bilinearly interpolated to the Wyscout pitch resolution. 

3. Player minutes data is sourced from Transfermarkt. Note: Transfermarkt does not count added time is not counted in the minutes played per game i.e. the max minutes played per match is 90, if a player is subbed on at the 90th, his playing time is logged as 1. 

#### Top JxT Pairs 

The table below shows the top 15 players in Europe’s top 5 leagues (Premier League, La Liga, Bundesliga, Serie A, Ligue 1) with the highest cumulative JxT throughout the season (at least 900 minutes together). It should be no surprise that the #1 pair is De Bruyne-Sterling, with Man City having the most impressive league season that year finishing with a 100 pts. Messi-Suárez are also fairly sensible runner ups, with the duo helping Barcelona to a nearly unbeaten domestic season. Payet-Thauvin were a little surprising to me at first, but I then recalled that Thauvin had a smashing season (22G,14A in 34 in the league) which earned him a call up to the French World Cup squad. Payet himself would’ve been in Russia too if not for an unfortunate injury during the 2018 UEFA Europa League Final.    




