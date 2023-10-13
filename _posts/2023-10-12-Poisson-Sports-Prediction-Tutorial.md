---
layout: post
title:  "Poisson Sports Prediction Tutorial"
author: Braden Critchfield
description: A short tutorial explaining how to use poisson distributions to calculate the probability of sport outcomes.
image: /assets/images/baseball2.jpg
---

When the improbable victory occurs, it feels magical. When the unlikely defeat happens, it feels tragic. The randomness of games and sports is part of what makes the enjoyable. Understanding this randomness and getting a sense as to how likely or unlikely something is to occur can help one better appreciate the feat one’s witnessed.
Using simple probability distributions, we can quickly determine the probability of a sporting event outcome. In this article, we will discuss the use of a Poisson distribution, which can be used in basketball, baseball, soccer, and football.

# What is the Poisson distribution?

The Poisson distribution gives the probability of an event occurring a certain number of times in a given amount of time or space. In the context of sports, this can be the number of points scored per minute, the number of runs scored per inning, etc. By using this distribution, it allows us to find the probabilities that a team will score x number of points/runs/goals in a k amount of time remaining. Then, by comparing this to the distribution of the other team, we can quickly find the probability of a team scoring more than the other team by the end of the game. 

# Putting it into Practice
## 1988 Kirk Gibson Home Run

For this example, we’ll look at baseball. In baseball, the unit of time is an out. Three outs make up an inning, and once a team gets 27 outs, they no longer have an opportunity to score runs. Because this is a simple model, we’ll ignore effects like opponent and pitcher, and assume independence between each occurrence.
On October 15, 1988, the Los Angeles Dodgers were playing the Oakland Athletics in the World Series. The Dodgers were down 4-3 in the final inning. This was the Dodgers final opportunity to win the first game of the Series. What was their odds of winning the game? Let’s use python to find out.
First, we need to import the correct packages. In this case, we’ll use poisson from scipy.stats.

```
from scipy.stats import poisson

```

Second, we need to find the average number of runs the Dodgers scored in 1988. According to baseball reference (https://www.baseball-reference.com/leagues/majors/1988.shtml), the Dodgers scored 3.88 runs per 27 outs in 1988. This means for every out, they scored on average .144 runs. With three outs left, they were expected to score .431 runs.
Poisson.cdf gives us the parameters k (the outcome we are finding the probability for) and mu (the average outcome in that amount of time). In this case, k is 2 (because the Dodgers needed 2 runs to win) and mu is .144. Because this will return the probability of scoring anything less than 2, we will subtract this number from 1 to get the probability of scoring anything more than 2. 

```
1- poisson.cdf(k=2, mu =.431)
```

In this case, the probability comes out to be 0.97%. As the legend goes, the Dodgers got a runner on base but spent two outs. They turned to their injured star, Kirk Gibson, to attempt to win the game. One strike away from striking out, Kirk Gibson hit a home run, winning the game for Dodgers, who went on to win the World Series. This becomes more impressive considering how unlikely it was for the Dodgers to win the game.

## Example 2: 1996 Utah Jazz comeback
Again, this is a simple model that doesn’t take into account broader context, but it can be helpful to determine a rough probability in a pinch. Take for example the greatest comeback in the National Basketball Association. 
The Utah Jazz were down 36 points halfway through a game in November of 1996. If you were a fan (or even alive) at this time, you probably knew I was extremely unlikely the Jazz would come back to win the game. But how unlikely? 

```
#Utah Jazz averaged 103.1 points per game
#Denver Nuggets averaged 97.8 points per game

#Generate list of points from 0 to 101 for the Nuggets and a list of each of 
#those numbers plus 36 for the Jazz. This is the outcomes where Jazz can win.
point_values_Jazz = list(range(0+36,101+36))
point_values_Nuggets = list(range(0, 101))

#Find the probability of the Nuggets scoring each of those point values.
#Find the probability of the Jazz scoring at least that many point values
UtahJazz = 1 - poisson.cdf(k = point_values_Jazz, mu =103.1/2)
DenverNuggets = poisson.pmf(k = point_values_Nuggets, mu =97.8)

#Multiply the two probabilities together to find the probabilitiy of each occuring (assuming independnece)
Deficitprobabilities = UtahJazz * DenverNuggets

#Sum up those probabilities to find the sum
Deficitprobabilities.sum()*100
```

According to our simple model, there was a .00000000028% of the Jazz winning. Talk about improbable!
To build more complex models, you can take into account other factors, such as defense of the opposing team, average when under similar factors, etc. However, a simple Poisson model works well in a pinch! Next time you watch a sporting event, try a simple poisson model to see who is favored to win!
