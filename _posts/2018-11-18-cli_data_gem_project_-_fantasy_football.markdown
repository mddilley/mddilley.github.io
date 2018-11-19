---
layout: post
title:      "CLI Data Gem Project - Fantasy Football"
date:       2018-11-19 00:52:43 +0000
permalink:  cli_data_gem_project_-_fantasy_football
---

For my CLI project, I decided to explore a topic that is close to my heart as the NFL seasons progresses - Fantasy Football. 

![](https://thesnapfootball.files.wordpress.com/2014/08/football-fantasy-gif.gif)

From the beginning, I decided that I would like to code an application that scrapes data from one of the popular Fantasy Football websites. I also decided that I would focus on scraping a list of top ranked players and then offer to show details about a specific player in that list. I am going to concentrate on the the ideation phase and refactoring phase in this post to offer up some insight into the design and revision of this application.

In Fantasy Football, a person chooses a starting lineup each week that includes the following positions: quarterback, running back, wide receiver, tight end, and kicker. A defense is also chosen, but I dropped this variable from the application since it is a collection of players instead of a single player like all of the position variables.

To begin planning my objects, I started with the obvious one: **Player**. A player has a name, team, position, Fantasy Football rank, and more. My next object that I identified was the **Scraper**. The Scraper object will serve to scrape the data from the chosen Fantasy Football website and to collaborate with the Player class to create Player instances with attributes scraped from the website. The last object that I decided to create is the command line interface. The **CLI** class will control the flow of the program and create new instances of the Scraper class based on inputs.

After creating a gem skeleton, I begin working outside-in with the CLI class. I began by identifying the choices that would be made in my control flow. The first option to the user would be which position for which to display the top ranked players (QB, RB, WR, TE, or K). Once the CLI displayed the top ranked player, it would then allow the option to see detailed information about each player on the list displayed. The choice would be made by selecting the rank number that corresponded with the desired player. The CLI would then allow the user to select another player from the same list or generate a new list for a different position. This would allow the user to navigate all options of position and player by position before ending the application.

After filling in the CLI class with fake data and the control flow, I began creating the Scraper and Player classes to replace the string outputs in the CLI with interpolated data. Thus began my first pitfall...

Hashes. I really wanted to intergrate metaprogramming into my CLI project. This led me down a path of using Nokogiri and OpenURI to scrape data and then store them in player hashes that would then be passed into a custom class constructor in the Player class. This custom class constructor used mass assignment to iterate through the hashes and create Player instances. To take it to the next level, I created another class method in the Scraper class that would create player hashes nested in an array. Another custom class constructor would accept the array of player hashes and use iteration and mass assignment to create all the players per position at once. This sounds really fancy, but, after some testing and thought, I realized that I over-designed the process, and I completely passed on the opportunity to truly harvest the power of object collaboration. Scraping all of the players at once also provided a very long load time. So, how did I fix this?

I took code that looked like this:

```
hash = { :name => doc.css('h1').text,	
         :position => doc.css('div .pull-left h5').text.strip.split(" - ")[0],	
         :projection => doc.css('.clearfix.detail span.pull-right')[2].text.split[0],	
         :team => doc.css('div .pull-left h5').text.strip.split(" - ")[1],	
         :height => doc.css('span.bio-detail')[0].text[8,6].strip,	
         :weight => doc.css('span.bio-detail')[1].text[8,3],	
         :age => doc.css('span.bio-detail')[2].text[5,2],	
         :college => (doc.css('span.bio-detail')[3].text.split(": ")[1].to_s rescue rescue_s)	
       }
```

And refactored it into this:

```
      player.projection = doc.css('.clearfix.detail span.pull-right')[2].text.split[0]
      player.team = doc.css('div .pull-left h5').text.strip.split(" - ")[1]
      player.height = doc.css('span.bio-detail')[0].text[8,6].strip
      player.weight = doc.css('span.bio-detail')[1].text[8,3]
      player.age = doc.css('span.bio-detail')[2].text[5,2]
      player.college = (doc.css('span.bio-detail')[3].text.split(": ")[1].to_s rescue rescue_s)
      player.scraped = "Y"
```

I also modified the Scraper class to only scrape what is needed. The first iteration of my Scraper involved scraping data for 20 players all at once. The refactored version only scraped data as needed which drastically reduced the amount of loading time. It also harvested the power of the Player class to store data immediately instead of relying on data constructs being passed around. Speaking of the power of objects, let's visit my next design pitfall...

Duplicate objects. While trying to go to bed one night, I realized another large problem with my CLI project. Within the control flow of the CLI, I was instantiating new objects without a care about duplication. The Scraper class served to come to life, do my bidding, and then hang out in memory with its clones. Each time the CLI looped back through the options, new Scrapers and Players were generated instead of using the existing Scrapers or Players already in existence. For example, if a user chose to see the top ranked quarterbacks, requested details of Player 1 and Player 2, decided to go back and request to see the top ranked quarterbacks and look at the details of the same two Players, then 2 Scrapers would exist in memory for the same position and four Player instances would exist (two objects for each of Player 1 and Player 2). This is no bueno.

My solution to these two problems were different but served the same purpose of preventing duplication of objects. For the Scraper, I built a custom class constructors that checked for an existing Scraper for a position, and, if so, it would return the existing Scraper object. If not, a new Scraper would instantiate to scrape for a position. For the player problem, I added an attribute that would serve as an "on/off" switch to prevent player duplication. This player attribute name "scraped" contains either nil (not scraped) or "Y" (scraped). The addition of attributes depends on the value of this variable, and, during the addition of attributes, the value is switched to "Y".

My last pitfall relates to switching gears when you have already built a lot of code that depends on a choice made long ago. Yep, my source, ESPN.com did not cooperate as well as I would have liked, and I had to switch my data source to Fantasypros.com halfway through my project. This took more of a mental toll more than anything else. Once I focused and powered through finding the correct CSS selectors to target all over again, my project was once again afloat. It was very satisfying seeing the rest of my project not break through the process of changing data source. This reinforced the priority of creating code that is open to change.

Through many head scratchings and regroupings, I completed my CLI project. This project taught me so much, and it forced me to learn (the hard way!) that some of my patterns of thinking were flawed and could be improved. My future plans with this project are to either expand on its data sources or create a test suite in order to practice creating tests in RSPEC.

