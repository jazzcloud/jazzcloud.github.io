# Lean Poker 
## Feedback & conclusions

I just came from the second Lean Poker event in my life. And I was facilitating it. I collected some feedback from participants, I'd like to share.

- All of the participants said that they would like to repeat it one day. 
- Thea mean answer on question "How likely would you recommend it to a friend(0..10)?" is 9.7!

Now to the bad part. 
- I realized that people was expecting something much more boring. 
- And initial description of event told people nothing about what is going to happen, who could benefit from it and what is required to participate.

So I had to fill these gaps.

- Your programming skills level doesn't matter. As soon as you can write a simple function in your favorite language and push commit to github, that is enough.
- but it does not mean that if you're super-experienced developer, you will not benefit from the workshop.

# The process.

Once we're finished with introduction words, people choose their language and form a teams.

After competition have started,  you're supposed to implement this function:

```ruby
def bet_request(game_state)
     
end
```

Depending on result of this function your bot is going to start winning or loosing.
Of course you need to read the API and set up the environment to be able to test it locally. But ask yourself, is there something you can do at this point to be better than my competitors.

Another challenging thing is the game structure. I hope this picture will make it a bit easier to understand:

![](picture.png)

So top of the picture demonstrates one game. Bots are betting, and opening the cards, then somebody makes the money, if he has better combination of cards or everybody else folded.

The tournament consist of lots of the games(until one player gets all the money). The winner gets 5? points. Second player gets 2? points. 
And on the bottom you see the graph, which is built from those points. And every point of the graph is a tournament.

Facilitator can change speed of the game, so each tournament can run every minute or every 10 seconds. Usually it is slower at the beginning of a workship, and faster at the end.



======
## What is this thing and why do I care?




I participated in Lean Poker event after CraftConference in Budapest.