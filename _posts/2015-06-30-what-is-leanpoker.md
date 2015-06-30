---
layout: post
title:  "What is Lean&nbsp;Poker"
date:   2015-06-30 18:02:42
categories: leanpoker
comments: true
---

# Workshop? Hackathon? Competition?

Official website says:

> "Lean Poker is a day long workshop, focusing on aspects of Lean Startup and Continuous Deployment"

But it has little in common with classic _workshops_, where participants learn new technology.

- There is no teaching, no lectures, no repeting after presenter. You already know enough, believe me.
- The goal is not to learn something new. The goal is to learn how to use what you already know more efficient.
- You will have to program poker bot. Your bot will start to fight with other since very first second. And you have limited time. Like in _hackathon_. 
- You will have 4-5 one-hour coding sessions with brakes to discuss your progress, current problems and solutions(like _agile retrospectives_).
- There will be you, your favorite language, your laptop, your team and very competitive environment.
- Yes, you can call it a _competition_. It is definitely a _competition_! But winning is not a goal.
- I personnaly like formulation "_startup conditions simulation_".

# What will I get/learn?

As a programmer, how fast do you think you're able to produce the value?
What is the value at all? How valuable unit tests are? Should you invest your time in clean code? Does selection of programming language matter?

These are good questions, isn't it? At the end of day you will not just know the answers. You will have deep level of understanding of those things.

# Who is it for?

Your programming skills level doesn't matter. As soon as you can write a simple function in your favorite language and push commit to github, that is enough to participate

# The process

Once we're finished with introduction words, people choose their language and form a teams.

There is one necessary function to implement. It accepts hash, which contains data representing current state of the game.

{% highlight ruby %}
def bet_request(game_state)
  ...
end
{% endhighlight %}

Depending on this data you need to return integer value, representing your current bet.

Of course you need to read the API and set up the environment to be able to test it locally. But ask yourself, is there anything to do to become better than your competitors even at this starting point.

# The structure
There is a game. Tournament consists of many games, and it continues until one player gets all he money. Winner takes 5 ponts, second place takes 2 points. 

Then new tournament starts. And then another and another during the whole day.
So don't worry that your bot losed all the money. Those are not real money anyway :) And it was just one tournamet - one small point on resulting graph. 

I hope this prezintation helps to understand the structure more than confuses:

<iframe id="iframe_container" frameborder="0" webkitallowfullscreen="" mozallowfullscreen="" allowfullscreen="" width="630" height="400" src="https://prezi.com/embed/oatjvg6lcjyv/?bgcolor=ffffff&amp;lock_to_path=0&amp;autoplay=0&amp;autohide_ctrls=0&amp;landing_data=eyJleHBlcmltZW50cyI6eyJjdGEiOlstMiwwXSwiZml0LWxvYWR1aSI6Wy0yLDBdfSwicGFnZV92aWV3X2lkIjoiZjlhYWQ0YjA5MDIxNWZlNyJ9&amp;landing_sign=3SLaemgUJE01HLtZNAje6UGPCPXL5D2RpGGOdjkIrDQ%253D#"></iframe>

Please let me know in comments if it doesn't makes sense.


# The roots

Lean Poker is invented by hungarian programmer Rafael Ordog. First Lean Poker event was held in January of 2014. 
Since then platform become more mature and stable, so now it is ready to spread across the globe.

# Further reading & exploration

- [Lean Poker website](http://leanpoker.org/)
- [Lean Poker rules](http://leanpoker.org/rules)
- [Upcoming events](http://live.leanpoker.org/events/live)
- [Facebook](https://www.facebook.com/leanpoker)
- [Twitter](https://twitter.com/leanpoker)

