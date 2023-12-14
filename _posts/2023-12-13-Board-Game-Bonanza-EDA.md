---
layout: post
title:  "Board Game Bonanza: EDA"
author: Braden Critchfield
description: An exploration of the top 1000 rated board games.
image: /assets/images/GameImage.jpg
---

In a previous post, I showed how we can collect data to find what makes a board game popular. In this post, we'll use python to explore that data.

As a reminder, all data comes from BoardGamesGeek!

![BGG logo](/assets/images/BGG.webp)

# What is in our data?
As you might remember from the [previous post](/_posts/2023-12-12-Board-Game-Bonanza-Data-Collection.md), this data consists of the top 1000 rated games on BoardGameGeek. The data also includes different variables about each board game, including year, age recommendations, estimated time, ratings, average price, etc.

Because this is the top 1000 board games and not all the board games (due to time constraints), we can't draw any specific conclusions about what makes a board game highly rated (since this iis a biased sample). However, we can still make some interesting observations.

# What are the distributional breakdowns of each factor in our data? What outliers are interesting?
First, let's take a look at the distributions of our data. This first figure shows the histograms for our three categorical variables: group size, length of time, and age rating. 

![CategoricalHistograms](/assets/images/CatHists.png]

The vast majority of these top 1000 games are in the center of these 3 groups: Small - Large groups (but not too small or too large), short to long (but not quick or too long), and are rated for preteeens and teens (not too young but not too adult either). Whether this is because there are just more of games with these factors in general, or because these receive higher ratings is the question that can only be answered by looking at more data.

We will also take a look at the distributions of our numerical data. Unfortunately, some of these graphs are impacted by outliers:

![Numerical Variables](/assets/images/ContHistplots1.png]
![More Numerical Variables](/assets/images/ContHistplots2.png]
![And More Numerical Variables](/assets/images/ContHistplots3.png]

I adjusted these graphs to account for outliers. I changed the year published distribution to leave out the 6 games that are pre-1950 (one game, Go, was from 2000 B.C.E. which made it difficult to see the graph). I removed the 4 games that have max players of 99-100 players. For the distribution of playing times, I removed two games with a playing time of 20 hours. The Average Price distribution has "Magic: The Gathering" left out, because it has an average price in the $7000s (probably due to the rarity of some cards). There are some other outliers I left in; for example, there are 11 games with over 80,000 ratings. But I removed the outliers that made the graphs difficult to see, and I wanted to note them because they are different than the vast majority of the games.

Some of my takeaways for these distributions and what they tell us about the most popular 1000 games:
1. The most popular games are all pretty recent; the median age is 8 years old, or around 2015.
2. It seems the most common number of players range is 2-4 players.
3. The median playing time is an hour and 15 minutes and the median age minimum is 12 years old.
4. While these games are sorted by top 1000 games by Bayes Rating, the better measure of popularity is number of ratings, which is the best indicator of whether someone played it.

# How are different variables related? And which variable relationshps should we ignore?
Next, let's take a look at how the variables are all related through a correlation plot.

![Correlation Matrix](/assets/images/correlationmatrix.png)

And here is a correlation matrix with a few outliers removed, which could skew the correlation:
![Correlation Matrix](/assets/images/correlationmatrixnew.png)

My biggest takeaways from this figure:
1. First is what to ignore: **Bayes and Average Rating** are highly correlated, which makes sense because they are the same except for an additional 30 average ratings for the Bayes rating. When performing analysis, it is best to ignore one of these values to avoid covariance.
2. As well, ignore the correlation between **Age and Year Published**; because one is a function of the other, these will always be perfectly negatively correlated.
3. **"Number of Ratings" and "Number of Accessories"** are pretty moderately correlated. Perhaps the more expansions and versions of a game, the more opportunities a person has to play the game and the more likely they are to rate the game.
4. There is a moderately high correlation between the **variance of ratings and the amount of playing time a game requires**; since a higher variance indicates a greater spread of opinion, this seems to show that the longer a game takes, the more ploarizing a game becomes.
5. There's an interesting slight negative correlation between **number of ratings and average price**. My prediction is that the more popular a game is, the more games there are which lowers the price. It's the games that are a little rarer but still have a devoted fanbase that are likely more expensive, wwhich is backed up by the fact that price and average rating are more highly correlated than price and Bayes rating (less ratings means the generic ratings in a Bayes Rating are weighted more heavily).
6. The **minimum number of players** has a very small correlation with the **maximum number of players**. This is very interesting because I assumed that if there is a smaller minimum, there will likely be a small maximum as well. There could just be noise throwing off the correlation in this case, but there's enough that makes the correlation near 0.

# How are number of reviews distributed across different factors? Is there any significant difference between their means?
When it comes to our categroical factors, I was interested to see if any of them had an impact on a game's popularity. I took a look using violin plots:

![Violin Plots](/assets/images/ViolinPlots.png)

These plots sort of parallel our distributions. That is, the more "central" values of these factors have the highest median number of ratins and the highest maximums (though it doesn't look like any of these are significantly different from one another). However, there are more popular games in the quick and young categroies than one would expect given their number in the dataset. Perhaps this is tied to people rating games they play, and many people play a lot of games when they were kids. Playing time and age minimum are moderately correlated due to the fact that younger kids have shorter attention spans.

# How has does year published impact popularity?
In looking at these top 1000 games, it's interesting to take a look at how when the game was published impacts the game's popularity (as measured by number of ratings).

![Year vs. Number of Ratings](/assets/images/PopularitybyYear.png)

The most popular games were published between 5-20 years ago (with one of the most popular being almost 30 years ago). On one hand, we shouldn't expect to see any extremely recent games have a lot of ratings, since ratings increase over time. But part of this could also be attributed to who is giving ratings and the response bias this creates.

From these graphs, it indicates that the highest rated games are those that are similar in basic features. What sets them apart is their content, game mechanics, and strategy.

Visit [my repository](https://github.com/bradenmcritchfield/semester_project/tree/main/DataProject) for data and jupyter notebooks to create these graphs.
