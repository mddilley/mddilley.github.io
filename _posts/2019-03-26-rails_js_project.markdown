---
layout: post
title:      "Wagger with JavaScript!"
date:       2019-03-26 21:49:49 -0400
permalink:  rails_js_project
---

To begin the next phase of my Ruby on Rails application, Wagger, I started by planning out how to fulfill the project requirements. As a refresher, the purpose of Wagger is to allow users to signup, add their dogs, and then coordinate dog play dates with other dogs. With many outlets for adding JavaScript frontend to my application, here were my starting points.

**The Plan**

1. Render a user's dog index page using AJAX and active model serialization in the Rails backend.

2. Render the dog show page with AJAX and active model serialization in the Rails backend.

3. Render a dog's play dates on the dog show page to display a `has_many` relationship from JSON rendered in Rails.

4. Render comments about a dog play date via a submission form and dynamically append comments to its show page.

5. Create Dog and PlayDate classes with prototype methods in JavaScript.

After creating this basic outline, I began working on the code, and, as seems to happen, some of the plans did not work out, but, that's okay, I'll share the end result!

The first hiccup was the realization that planning a whole new model (Comment) would not be the best focus of energy for a project that was all about adding JavaScript to an existing project. So, my goal for requirement #4 was out. What did I come up with instead? I decided to dynamically add play dates from a form that would show directly on the play date index page. This ended up becoming a great exercise in not only DOM manipulation using jQuery but also making post requests to my Rails API. 

I ended up with an index page that has a "Create a New Playdate" button that reveals the new playdate form. After the form is revealed, the user can then fill out the form and submit or press a "Cancel" button to hide the form once again. Upon successful submission, the form hides, an AJAX post request supplies the serialized data to the Rails backend which then persists to the database and renders a JSON response which is then appended to the list of playdates on the index page. All without a page load! 

This creates a much better feel than clicking "Create a New Playdate", **loading a new page**, filling out the new playdate form and submitting, **loading the next page**, viewing the playdate show page and clicking on another link to see the playdate index page, **loading....** you get the idea! These small changes made a verifiable improvement in the user experiencing (unless you like your browser locking up and watching pages load...). I don't know about you, but that gets me excited!

![](https://media.giphy.com/media/oBPOP48aQpIxq/giphy.gif) *Chuck Norris approves*

Integrating JavaScript in my project provided several "think outside the box" moments when I realized that I could not always rely on Rails macros in my JS code. This really pushed me to see the full stack of the project at hand.

**The Full Stack**

My Rails-only version of Wagger relied heavily on checking the whether the user logged in had ownership of the dog or playdate being served up in the view. Luckily, I created a helper in my ApplicationController called `current_user`. This method was very handy when writing ERB not so much when in the the thick of JavaScript. How did I bridge the gap? There are many solutions to this problem, but I decided on calling  `#current_user` in my /layouts/application view and embedding it in the body tag. This would make the current user id available to me in every view after a user logged in. Here is the code:

*In application_controller.rb,*
```
def current_user
    @current_user ||= User.find(session[:id])
end
```
*In /layouts/application.html.erb,*
```
<body data-user-id="<%= current_user.id if logged_in? %>">
```
*In play_date.js,*
```
$('body').data("user-id")
```
Using this path, I could write JavaScript code that depended on knowing if the current Dog or PlayDate instance (in JS) had a `belongs_to` relationship with the current user logged in.

**Navigating Differences in Conventions**

Another moment where the full stack mindset crept in was considering the differences in naming conventions between Ruby and JavaScript. For example, in Ruby, a variable name containing multiple words is named using snake case (`my_dog`) whereas the same variable would be named using camel case in JavaScript (`myDog`). This could be addressed in either the property assignment for the JS class or the active model serializer on the backend. I chose the Rails side to explore a bit more about Rails serializers. I found that attribute names of the serializer class can be altered and defined as  instance methods of the class. Here is an example:

*In dog_serializer.rb,*
```
attributes :id, :friendlyRating

  def friendlyRating
    object.friendly_rating
  end
```
In this code, the attribute of the Dog model, friendly_rating is called on the object (implicit self) to expose the value of friendly_rating for an instance. This value becomes the return value of #friendlyRating of the DogSerializer. As a result, the JSON response of a rendered instance of the Dog model contains `friendlyRating` instead of `friendly_rating` as seen below.
```
{
   id: 8,
   name: "Benny Boy",
   age: 10,
   breed: "Pembroke Welsh Corgi",
   weight: 24,
   fixed: true,
   userId: 14,
   img: "bennyboy.jpg",
   friendlyRating: 10,
   aggressiveRating: 2,
   sex: "Male",
   playDates: []
}
```

This project provided many other challenges including converting time and date formatting between Rails and JavaScript (solved with moment.js), recreating a lot of the frontend styling with JavaScript (solved using Handlebars.js templates and helpers), and recreating the removal of dogs from playdates using JavaScript. All of these challenges afforded me the opportunity to not only expand my skillset but also to look at features and solve problems through the lens of JavaScript. 


