---
layout: post
title:  "Board Game Bonanza: Data Colleciton and Wrangling"
author: Braden Critchfield
description: A short tutorial explaining how to collect and clean data relating to board games.
image: /assets/images/baseball2.jpg
---

Everyone loves a good board game. Board games bring people together, offer intellectual stimulation, and provide a form of entertainment that is not tied to a screen. But what makes a board game popular? 

To explore this question, first we need to find data on popular board games.

# Data Source
One great source of board game data is [BoardGameGeek](https://boardgamegeek.com/). This website is an online database and community for those who love board games. Not only does it have information about hundreds and thousands of board and card games, but it also allows users to rate and sell board games on the website. Because of this, we can find data about each board game's requirements, it's popularity, and it's value.

BoardGameGeek offers an api that allows the user to access data on a game by game basis. Before accessing the api, it's best to check the [Terms of Service](https://boardgamegeek.com/wiki/page/XML_API_Terms_of_Use#). The good news is that we are able to use this data for non-commercial use. We just need to credit Board Games Geek by including its logo in public facing uses of the api.

So all credit for this data is given to BoardGameGeek. Here is their logo:

![BGG logo](/assets/images/BGG.webp)

## Accessing the API
Board Game geek has [an explanation of their api](https://boardgamegeek.com/wiki/page/BGG_XML_API2).

To access the data for each board game, we need the base url, the endpoint, parameters (an id and set the api to show stats and marketpace data).
```
baseurl = "https://boardgamegeek.com/xmlapi2"
endpoint = "/thing?id="
parameter1 = str(ids[0])
parameter2 = "&stats=1&marketplace=1"
```

Each api pull will result in an XML file, that looks a little like this:
![XML api pull example](/assets/images/Sample XML file.png)


## Pulling information from the XML files
7. download CSV file to get IDs
8. code to scrape through top 1000 IDs

# Data Cleaning and Engineering
10. Show and explain code
