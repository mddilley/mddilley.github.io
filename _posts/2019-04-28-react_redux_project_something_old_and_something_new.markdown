---
layout: post
title:      "React/Redux Project: Something Old and Something New"
date:       2019-04-28 19:59:58 +0000
permalink:  react_redux_project_something_old_and_something_new
---


For my final project using React and Redux, I decided to create a basic version of software that I have used in many laboratories that is crucial to maintaining and distributing the product of production laboratories, a laboratory information managment system (LIMS). The purpose of my project, Oculus LIMS, is to allow a user to batch laboratory samples and assign unique identifiers to both the batch and samples contained in the batches. 

My first challenge was identifying my containers. I isolated two models for my backend that would also serve as containers in the React/Redux frontend: batch and sample. I planned the tree for React components as follows.

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


