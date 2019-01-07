---
layout: post
title:      "Sinatra Project: Magic the Gathering Deck Builder"
date:       2019-01-07 18:35:26 +0000
permalink:  sinatra_project_magic_the_gathering_deck_builder
---


For my project using [Sinatra](https://github.com/sinatra/sinatra) (a domain-specific language used to quickly develop web applications in Ruby), I decided to dive back into an old hobby of mine: Magic the Gathering.

![*Fly me to the web...*](http://hermanleonard.com/graphics/images/_big/FRS07.jpg) *Fly me to the web...*

I began my project by creating a project folder with the structure below.

```
-root folder
  |-app
	|  |-models
	|  |-views
	|  |-controllers
	|-config
	|-db
	|-public
```
	
Next, I focused on how I wanted my app to start. Since Sinatra is [Rack](https://github.com/rack/rack)-dependent, I created a config.ru file to mount my controllers. When all is said and done, I wanted to be able to type `rackup config.ru` to start my application server.

I also wanted to use Bundler to load all of my dependencies so I added that requirement to my environment. After getting my structure and endpoint figured out, I focused on planning my **models**.

In the application, I wanted a user to be able to login and create decks that contain cards. That previous sentence has three objects in it: **users, decks, and cards**. I created these three models in `/app/models` and then began thinking about the relationships between these three objects. Luckily, by using Sinatra, I already had the power of [ActiveRecord](https://github.com/rails/rails/tree/master/activerecord) on my side. ActiveRecord contains many association macros that build connections between objects and actively manage them in a SQL database (more about the database in a bit). To begin, I knew that users would have many decks and have many cards. These cards would be associated to the user through decks that they will build. A deck belongs to a user and has many cards. Card objects should be able to have many decks since decks will share common cards. I decided to build one more class, CardDeck, to flesh out this association between decks and cards. CardDeck allows the removal of cards from a specific deck without deleting the card objects. The association between a single card and a single deck is removed instead.

My next step was to create SQL tables that corresponded with each class that I created. I defined all of the class attributes in the following schema.

```
ActiveRecord::Schema.define(version: 2018_12_29_164446) do

  create_table "card_decks", force: :cascade do |t|
    t.integer "card_id"
    t.integer "deck_id"
  end

  create_table "cards", force: :cascade do |t|
    t.string "name"
    t.string "mana_cost"
    t.string "card_type"
    t.string "power"
    t.string "toughness"
    t.string "rarity"
    t.string "expansion"
    t.string "card_text"
    t.string "colors"
    t.string "flavor_text"
    t.string "img_url"
  end

  create_table "decks", force: :cascade do |t|
    t.string "name"
    t.string "size"
    t.string "color"
    t.integer "user_id"
  end

  create_table "users", force: :cascade do |t|
    t.string "username"
    t.string "email"
    t.string "password_digest"
  end

end
```

After building out my schema, I migrated all the changes into my database, and it was ready to start storing data. The next step was routing all of the HTTP requests in my app. I used REST (representational state transfer) conventions to route the user through user signup/login, decks, and cards. I defined the routes in four different controllers: ApplicationController (defines the root index route, configures sessions, and also defines helper methods), UsersController (controlling signup/login/logout and authenticating user using the [Bcrypt](https://github.com/codahale/bcrypt-ruby) gem), DecksController (controls **C**reate**R**ead**U**pdate**D**elete of decks, **C**reates cards and removes them from decks, and enforces users only being able to modify their own decks), and CardsController (routes card views, **D**eletes cards, and enforces users only being able to delete their own cards).

After the views were built out, I used [Bootstrap](https://getbootstrap.com/) to bring some life into my application and make it responsive so it can be viewed and used on mobile or desktop devices. The last step that I plan on building out is using [Sinatra-Flash](https://github.com/SFEley/sinatra-flash) to create error messages to accompany the re-routes that I created to prevent duplicate user, card, and deck creation.

All in all, this project was immensely satisfying. A month ago, I was envisioning what my first web application using Sinatra would look like, and, now, I know. I look forward to continuing my work on it and improving it.
