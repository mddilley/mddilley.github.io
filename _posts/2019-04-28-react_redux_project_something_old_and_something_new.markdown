---
layout: post
title:      "React/Redux Project: Something Old and Something New"
date:       2019-04-28 15:59:59 -0400
permalink:  react_redux_project_something_old_and_something_new
---


For my final project using React and Redux, I decided to create a basic version of software that I have used in many laboratories that is crucial to maintaining and distributing the product of production laboratories, a laboratory information managment system (LIMS). The purpose of my project, **Oculus LIMS**, is to allow a user to batch laboratory samples and assign unique identifiers to both the batch and samples contained in the batches. 

My first challenge was identifying my containers. I isolated two models for my backend that would also serve as containers in the React/Redux frontend: **batch** and **sample**. I planned the tree for React components as follows.

```
<App />
 |
 -----<Welcome />
 |
 -----<About />
 |
 -----<BatchesContainer />
 |                  |
 |               <Batches />------<BatchesInput />
 |                  |
 |                 <Batch />
 |
 -----<SamplesContainer />
                    |
                 <Samples />------<SamplesInput />
                    |
                  <Sample />
```

After outlining this tree, I divided and conquered by fleshing out all the Batch components first and the using them as a model for the Sample containers. Once the React skeleton was written, I began incorporating Redux to maintain state in the application. 

The core concepts of Redux are defining **actions**,

```
export function addBatch(batch) {
  return { type: 'ADD_BATCH', batch}
}
```

defining **reducer functions**, and **updating state**.
```
manageBatch(state = {batches: [], loading: false}, action){
  switch(action.type){
    case 'ADD_BATCH':
      console.log("inside ADD_BATCH case in reducer")
      const batch = action.batch.attributes
      return {...state, batches: [...state.batches, batch]}
		default:
		  return state;
}
``` 

One issue that I ran in pretty quickly once my samples portion of the application was developed was a very **crowded actions file**. I fixed this issue by splitting my actions to be dispatched for batch state updates and sample state updates into separate files. By organizing the actions for these two separate components, I was able to clear my thought process when writing the next and more complex steps for each component (like the asynchronous actions that would later persist and fetch data to and from the Rails API).

Since this project was an opportunity to branch out, I decided to using the **[Netflix / FAST JSON API](https://github.com/Netflix/fast_jsonapi)** for data serialization in the Rails backend. I heard about this gem (along with Graphiti) at a Ruby meetup here in Austin, and I wanted to give it a shot. It ended up working out great for planning the structure of the JSON coming back to the frontend to render in the view. One cool feature that comes along with the FAST JSON API gem is key transformations. In my previous Rails and JavaScript project, I wrote code to maintain appropriate naming convention of attributes in the Rails backend and the JS frontend. FAST JSON API provides the option to transform all keys coming out of the backend from snake_case (Ruby convention) to camelCase (JS convention). I then found a package called **[Snake Case Keys](https://www.npmjs.com/package/snakecase-keys)** to use in the frontend to convert keys to snake_case. These tools helped keep the communication between frontend and backend predictable and tidy.

Another frustration that came along with navigating the stack in this project were **CORS errors**. Right when I finished my first code that connected the frontend to my API, I found this strange error that I immediately recognized from chatter on the Ruby on Rails Slack (an issue that no one was enjoying...). It seems scary at first. What is CORS!? Why is it sabotaging my "Eureka!" moment of seeing my code work?!

So, CORS stands for Cross origin resource sharing. This is the mechanism that filters requests to a server originating from somewhere else. It makes sense when you think about it. We want to control what resources can successfully request data from our server. After a bit of research, I realized that resolving this error boiled down introducing a gem into my gemfile and then specifying the origin of the request that I wanted to succeed.

Lucky for me, the backend project started with the following command that made my life easy when it came to the CORS error.

```
rails new <project name> --api
```

Starting a Rails project with this command starts you off with a gemfile that contains `gem '[rack-cors](https://github.com/cyu/rack-cors)'` that needs to be uncommented (then run `bundle install` in the terminal). This gem provides middleware that allows cross domain AJAX calls. Next, to configure the middleware, uncomment and edit the following lines in `config/initializers/cors.rb`.

```
Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins 'localhost:3000' // add appropriate port here for the originating request

    resource '*',
      headers: :any,
      methods: [:get, :post, :put, :patch, :delete, :options, :head]
  end
end
```

This project was immensely satisfying, and I felt accomplished after executing my strategy to plan the structure, mock out the basic components, and then add features. This incremental process felt great, and I saw my application coming together with minimal hiccups, My future plans for this project are to incorporate user login and admin features, barcode generation for the batch and sample identifiers, and to create a add/remove tests for result tracking. After my Rails/JS project and this project, I have really come to enjoy the "call and response" feel of creating and connecting a frontend with a backend and seeing a complete process come to fruition.







