---
layout: post
title: Liverpool Analytics Challenge
description: >
  Praising the bitter rivals, with numbers.
author: author1
image: /assets/img/2020-07-20-Liverpool-FoT/trent_reuters.png
noindex: true
categories: [football]
tags: [Football, Premier League, Analytics]
---

A while ago I decided to have a go at the Liverpool Analytics Challenge organised by the [Friends of Tracking](https://www.youtube.com/channel/UCUBFJYcag8j2rm_9HkrrA7w) team. 

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Ready for a challenge? We have been provided tracking data for 19 Liverpool goals from 2019 by <a href="https://twitter.com/lastrowview?ref_src=twsrc%5Etfw">@lastrowview</a>. The <a href="https://twitter.com/hashtag/FoT?src=hash&amp;ref_src=twsrc%5Etfw">#FoT</a> competition is to do a data-driven analysis of how The Reds score. Deadline 11th May. Start today. <a href="https://t.co/aqja37cy5z">https://t.co/aqja37cy5z</a></p>&mdash; David Sumpter (@Soccermatics) <a href="https://twitter.com/Soccermatics/status/1255024883836366849?ref_src=twsrc%5Etfw">April 28, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

The biggest novelty of the challenge was perhaps the provision of tracking data for 19 Liverpool goals provided by [@lastrowview](https://twitter.com/lastrowview). Before this, I believe the most detailed publically available 
datasets were the event-level datasets provided by Statsbomb and Wyscout. There was also data provided for the PSG Sports Analytics Challenge but I haven't checked if that data is stil hosted online. 

Anyway, the goals of the challenge were to: 

1. Use one or more of the tools taught as part of the Friends of Tracking series (pitch control, speed and acceleration, passing networks, pass maps etc.) to analyse the data.

2. If you wish, combine your analysis with other data available from other sources on Liverpool.

3. Produce an output (short report/video) that can be commicated either to a coach, a video analyst or players.

I had pretty much one weekend to produce something before the deadline so yep I was pretty much glued to my monitor for that whole weekend. The rush of producing a deliverable did
bring back some memories of previous all-nighter and deadline cramping experiences at hackathons, but thankfully not too much sleep was sacrificed this time. 

The most substantial tool taught by Friends of Tracking at that point was probably the Pitch Control model, conceptualised by William Spearman (now chief data scientist at Liverpool)
et al at Hudl as a model for ball	control	in football	based	on how long	it takes a player	to reach the ball (time-to-control)	and	how	long it	takes	a	player to	control	the	ball (time-to-control).	

![PL 1920](/assets/img/2020-07-20-Liverpool-FoT/Pitch_Control.PNG)
Source: Physics-Based	Modeling	of	Pass	Probabilities	in	Soccer, Spearman et al., 2017
{:.figure}

Whilst I was working on the initial parts of my project, I had come across many neat animations and analyses of pitch control by other twitter users taking part in the challenge. 
I decided to take a slightly different approach for my submission by seeing if static visuals of pitch control change (gain) can still tell a good amount of information about a sequence of play (goals scored in these instances). 
For each sequence of events, I computed the pitch control values per 10 frames and simply took the linear regression gradient at each grid point. An example of the end result using Salah's goal against Man City this season is shown below. 

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">Salah’s header to make it 2-0 back in November. The successful long diagonal spearheads a surge in Liverpool’s pitch control into right midfield. By the time of Salah’s shot, Liverpool have made gains all across City’s box thanks to the runs of Liverpool’s 3 forwards. <a href="https://twitter.com/hashtag/FoT?src=hash&amp;ref_src=twsrc%5Etfw">#FoT</a> <a href="https://t.co/3AZlBxSRzg">pic.twitter.com/3AZlBxSRzg</a></p>&mdash; Gerald L (@dlareg49) <a href="https://twitter.com/dlareg49/status/1259452693866639360?ref_src=twsrc%5Etfw">May 10, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

{"height": 720, "html": "<iframe class=\"streamable-embed\" src=\"https://streamable.com/o/03abf\" frameborder=\"0\" scrolling=\"no\" width=\"1280\" height=\"720\" allowfullscreen></iframe>", "provider_name": "Streamable", "provider_url": "https://streamable.com", "thumbnail_url": "//thumbs-east.streamable.com/image/03abf.jpg?height=300", "title": "sala", "type": "video", "version": "1.0", "width": 1280}


Using this method, I then compiled the same plots for the other goals in the dataset (the PC function failed to converge for 3 goals, didn't manage to investigate why), thinking that from this collection of pitch control gain plots 
we can maybe identify certain characteristic "fingerprints" of how Liverpool score their goals. 

![PL 1920](/assets/img/2020-07-20-Liverpool-FoT/Liv_PCG_goals.PNG)

While the size of the dataset is too small for me to robustly infer anything, I think the main impression that I got was that Liverpool tend to gain the most control relatively evenly down the flanks during their successful attacking sequences.


All in all, while I didn't manage to come up with something truly novel for this competition, this was definitely a fun and fruitful exercise and I'm thankful to Friends of Tracking, Last Row and Metrica Sports for giving me 
and other budding enthusiasts the opportunity and resources to mess around with some highly coveted football tracking data. I wished to keep this post short but I think it's gone on longer than I anticipated initially already, so I'll just leave 
my submitted presentation (which I modelled after an opposition "primer" of sorts) below for your viewing. Til next time ;) 

<iframe src="https://docs.google.com/presentation/d/e/2PACX-1vRXRtQFpr9TyFoVzQGXNUuDGrFuw-e8D2ndOwvkG2V-ptoImHaFr0lHtl3FS0G1tg/embed?start=false&loop=false&delayms=3000" frameborder="0" width="640" height="389" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>

