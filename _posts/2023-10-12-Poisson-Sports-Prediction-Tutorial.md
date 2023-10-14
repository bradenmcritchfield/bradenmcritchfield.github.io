---
layout: post
title:  "Predictions in Sports using Poisson Distributions"
author: Braden Critchfield
description: A short tutorial explaining how to use poisson distributions to calculate the probability of sport outcomes.
image: /assets/images/baseball2.jpg
---

When the improbable victory occurs, it feels magical. When the unlikely defeat happens, it feels tragic. The randomness of games and sports is part of what makes them enjoyable. Understanding this randomness and getting a sense as to how likely or unlikely something is to occur can help one better appreciate the feat one’s witnessed.
Using simple probability distributions, we can quickly determine the probability of a sporting event outcome. In this article, we will discuss the use of the Poisson distribution to determine probabilities in sports like basketball, baseball, soccer, and football.

# What is the Poisson distribution?

The Poisson distribution is a statistical distribution that uses the average occurences in a given amount of time or space to give the probability of an event occurring a certain number of times in that same amount of time or space. In the context of sports, this occurence can be the number of points scored per minute, the number of runs scored per inning, etc. By using the poisson distribution, it allows us to find the probabilities that a team will score x number of points/runs/goals in a k amount of time remaining. Then, by comparing this to the distribution of the other team, we can quickly find the probability of a team scoring more than the other team by the end of the game. 

# Putting it into Practice
## Probability for 1 Team: 1988 Kirk Gibson Home Run

For this example, we’ll look at baseball. In baseball, the unit of time is an out. Three outs make up an inning, and once a team completes 9 innings, they no longer have an opportunity to score runs. This is an imperfect unit of time since the length of time between each out can differ, but for a simple example it can work. Because this is a simple model, we’ll ignore effects like opponent and pitcher, and assume independence between each occurrence.
On October 15, 1988, the Los Angeles Dodgers were playing the Oakland Athletics in the World Series. The Dodgers were down 4-3 in the final inning. This was the Dodgers final opportunity to win the first game of the Series. What was their odds of winning the game? We can use python to find out.
First, we need to import the correct packages. In this case, we’ll use poisson from scipy.stats.

```
from scipy.stats import poisson

```

Second, we need to find the average number of runs the Dodgers scored in 1988. According to [baseball reference](https://www.baseball-reference.com/leagues/majors/1988.shtml), the Dodgers scored 3.88 runs per game (or 27 outs) in 1988. This means for every out, the Dodgers scored on average .144 runs. With three outs left, they were expected to score .431 runs.
Poisson.cdf requires the parameters k (the outcome we are finding the probability for) and mu (the average outcome in that amount of time). It then returns the probability of getting k or less. In this case, k is 2 (because the Dodgers needed 2 runs to win) and mu is .144. Because this will return the probability of scoring anything less than 2, we will subtract our poisson.cdf function from 1 to get the probability of scoring anything more than 2. 

```
1- poisson.cdf(k=2, mu =.431)
```

In this case, the probability comes out to be 0.97%. What actually happened was just as incredible. The Dodgers spent two outs and got one runner on base. They turned to their injured star, Kirk Gibson, to attempt to win the game. Literally hobbling up to bat against the Athletics' best pitcher, Kirk Gibson hit a home run and won the game for Dodgers, who went on to win the rest of the World Series. [Watching Kirk Gibson's at bat unfold](https://www.youtube.com/watch?v=Db2sFZVGxJ4), the 1% chance of the Dodgers winning feels like magic.

## Probability for 2 Teams: 1996 Utah Jazz Comeback
The Dodgers example only involves one team; how do we approach comparing two teams? To solve this, we'll look at the greatest comeback in the National Basketball Association. 
The Utah Jazz were down 36 points halfway through a game in November of 1996. If you were a fan (or even alive) at this time, you probably knew it was extremely unlikely the Jazz would come back to win the game. But how unlikely? 

Using [Basketball Reference's 1996-1997 team data](https://www.basketball-reference.com/leagues/NBA_1997.html), we find that the Utah Jazz averaged 103.1 points per game that season and the Denver Nuggets averaged 97.8 points per game. For simplicity sake, we'll ignore team defense (like most NBA teams ;) ).

First, we'll generate a range of values for the Nuggets that captures all values they could reasonably score in half of a game. We'll do the same for the Jazz, except a minimum and maximum 36 more than the Nuggets.

```
#Utah Jazz averaged 103.1 points per game
#Denver Nuggets averaged 97.8 points per game

#Generate list of points from 0 to 101 for the Nuggets and a list of each of 
#those numbers plus 36 for the Jazz. This is the outcomes where Jazz can win.
point_values_Jazz = list(range(0+36,101+36))
point_values_Nuggets = list(range(0, 101))

```

We'll use these range of values and Poisson.pmf to generate the probability the Nuggets will score each value in the range. We'll use 1 - Poisson.cdf to find the probability the Utah Jazz will score that value or more. 

```

#Find the probability of the Nuggets scoring each of those point values.
#Find the probability of the Jazz scoring at least that many point values
UtahJazz = 1 - poisson.cdf(k = point_values_Jazz, mu =103.1/2) #divide mu by 2 so it's average for the half
DenverNuggets = poisson.pmf(k = point_values_Nuggets, mu =97.8/2)

```
Because the Utah Jazz's list of probabilities is for values incremented 36 points more than the Nuggets', by multiplying each of these lists together, t gives us the probablity of the Nuggets scoring that number of points AND the Jazz scoring 36 points or more (assuming independence for simplicity). By summing these probabilities up, we can find the probability of any of these outcomes occurring. 

```

#Multiply the two probabilities together to find the probabilitiy of each occuring (assuming independnece)
Deficitprobabilities = UtahJazz * DenverNuggets

#Sum up those probabilities to find the sum
Deficitprobabilities.sum()*100
```

According to our simple model, there was a .03% of the Jazz winning. Talk about improbable!

# Conclusion
To build more complex models, you can take into account other factors, such as defense of the opposing team, average when under similar factors, etc. However, a simple Poisson model works well in a pinch! Next time you watch a sporting event, try a simple poisson model to see who is favored to win, or to see the probability a team holds on to their lead throughout the game. Because this model applies to any sport where occurrences occur in an amount of time, this works for soccer, baseball, basketball, football, hockey, and more. As you put this into practice, you can quantify the highs and lows of following sports by capturing how improbable it was!


