---
layout: post
title:      "Ruby on Rails: Getting Hooked"
date:       2019-02-15 10:58:13 -0500
permalink:  ruby_on_rails_getting_hooked
---


A couple of weeks before I completed arrived at project time, I came up with a clear idea of what my RoR project would explore. My idea came to me when I looked down at the small bed beside my desk. A long, four-legged creature stared back at me. "I'll make a dog play date application."

To start my project, I came up with the initial structure that included four models: **User** (to log visitors in and out, to allow a visitor to store their dogs, and to allow visitors to see available playdates and the playdates they will participate in with their dogs), **Dog** (to model a user's dog pack and their attributes), **PlayDate** (to store attributes of the playdate like location, name, time, and date), and **DogPlayDate** (to serve as a join tables between dogs and playdates). 

After fleshing out my models, I decided on the relationships between the models and then created the database migrations with all of the attributes for the models. 

For **User**, I decided on
```
has_many :dogs
has_many :play_dates, through: :dogs
```

For **Dog**, 
```
belongs_to :user
has_many :dog_play_dates
has_many :play_dates, through: :dog_play_dates
```

For **PlayDate**, 
```
has_many :dog_play_dates
has_many :dogs, through: :dog_play_dates
``` 

For **DogPlayDate**, 
```
belongs_to :dog
belongs_to :play_date
``` 

After defining the associations, I noticed one problem down the line while working on this application. I wanted a user to be able to cancel (delete) a playdate that they created. I did not want a user to be able to cancel playdates that they did not create. In order to make this change, I add a new association between **PlayDate** and **User** and ended up with the following code for **PlayDate**.

```
belongs_to :user
has_many :dog_play_dates
has_many :dogs, through: :dog_play_dates
```

By making this change and also adding a user_id column to the play_dates table, I was able to easily differentiate which user created which playdate. This small change greatly increased the features that I added to this project, and others that I plan to add in the future.

One of the hardest parts of this project was keeping it within scope. I remember reading about a little mischievous creature known as scope creep. My project's scope creeped out a bit here and there was I became more familiar with the patterns in Rails. My takeaway from this project was to set strong boundaries early and see them through. Luckily, creep only led to integrating a JSON of dogs breeds that I found on GitHub into my breed selection menu and a couple of methods that display dog's friendly and aggressive ratings with icons instead of plain old text. Rails is so versatile that I can now see how a project can take the long way around to the ultimate goal. I'll keep that lesson close as I continue towards larger projects.

Another surprise I encountered was how the convention-based patterns in Rails grew more familiar with time. At first, the magic of Rails can be intimidating. Many things happen out of sight, but, if you dig a little and read some documentation, the magic remains magical but not also grounded in real code. Macros like `validates` in models and `before_action` in controllers allow adding so much logic with a small amount of code, but they do come with their own challenges.

After working through this project, I see so many possibilities opened up by using Rails. I can't wait to start integrating JS with Rails for my next project and then continue on.











