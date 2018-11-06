---
layout: post
title:      "Collaborating Objects Pt. 1"
date:       2018-11-06 12:34:22 -0500
permalink:  collaborating_objects_pt_1
---

I've been waiting for a concept to trip me up, and collaborating objects did just that - and more! I've spent quite a bit of time over the last couple of weeks allowing this subject to seep into my brain. Time has helped a lot. Slowing down and giving myself plenty of moments to reflect on the labs, videos, and reading material in this course helped immensely. I found myself reading other blog posts about this concept as well (thank you, fellow strugglers!). 

![](https://media.giphy.com/media/l46Cbqvg6gxGvh2PS/giphy.gif)**Mood*

In order to further cement these ideas in my head, I thought it would be a great idea to write out a summary of them in blog form. So, without further delay, I'll start with an important concept of this post...

**Class**

A class is an object. Ok, there is more to it than that, but this is an important part of its identity. This object serves two purposes that will forever be implanted in my mind. A class is is both a *blueprint* and a *factory*. How is it a blueprint? It creates a template for what a class object can do and what purpose it serves. How is it a factory? A class has the ability to instantiate. I love the baby analogy for class. A class tells us what a baby can do (blueprint), and it can birth a baby (factory)!

![](https://media.giphy.com/media/tfduyLm5cufU4/giphy.gif)**Tommy and Dancing Baby (instances of Baby class)*

Ok, so you might be confused because those are both really outdated references, but you should really be wondering - what is an *instance*?

**Instance**

An instance is a new baby made from the factory! The blueprint tells us what the bright, shiny, and new baby can do. An instance is an object made in the image of the class. Ok, so now we have a baby, how does it do things?

**Instance Methods**

The methods of an instance are an encapsulation of logic just like all other methods. Instance methods are defined within the definition of a class. The two instance methods that first come to my mind are instance readers ("getters") and instance writers ("setters"). These two methods seem basic, but they allow two very powerful things to happen. They allow the programmer to store data within an object that is an instance of a class, and they allow the programmer to expose data that is stored within an instance. These two methods allow an instance of a class to have attributes like age, name, eye color, or temperment. Instance methods tell us what an instance of a class can do. Ok, but where is that data stored?

**Instance Variables**

An instance variable is a variable whose scope is instance scope. This means that data can be stored or accessed in the variable from the instance only. The syntax for this type of variable is `@variable_name`. This data can be casted to a higher scope if needed. Instance variables will store data about the attributes of the instance within the instance object itself. These variables can be modified through the setter/getter methods described above. Furthermore, the macros known as `attr_accessor` (a combination of setter and getter), `attr_reader` (getter), and `attr_writer` (setter) can be used to abstract away from the repetitive code for reader/writer methods.

For example:

```
attr_accessor :name
```

is equivalent to:

```
def name(name) # instance reader (getter)
   @name
end

def name=(name) # instance writer (setter)
   @name = name
end
```

Which would you rather use?

Alright, now we have some great functionality within class instances, but what is going on with the class itself?

**Class Methods**

A class can have its own methods which are known as class methods. The two that jump out at me the most are class constructors and class readers. A class constructor is a powerful method that allows us to create a new instance. It is the factory for new babies. `Class.new` runs through the code in an instance method known as initialize (an example of a hook, lifecycle event, or callback) and creates a new object in memory. Initialize can take in argument which will be expected when calling .new, but custom constructors can also be created to extend the functionality of the class constructor. For example, you may want to name a baby right off the bat, but you may also want to take some time to see if that baby looks like a "Bob" or "Steve." A custom class constructor could allow a programmer to name the baby right away by providing a name argument when called. The second class method that came to mind is a class reader which allows the programmer to expose data stored in the class scope. `self.all` is an example of a class reader that exposes data in a class variable that stores all instances of a class. It also harvests the power of the `self` keyword. I'll touch on that later, but, now...

**Class Variables**

Class variables hold data and have class scope. The syntax for class variable is `@@variable_name`. Class variables are useful to store instances of classes. For example, a programmer could define `@@all = []` within the class definition and then add `@@all << self` to the initialize instance method (here is that `self` keyword again). These two lines of code allow the programmer to store each instance object of a class in the array, `@@all`. As mentioned above, the `self.all` method could then expose the data stored in the `@@all` class variable. This is useful in tracking what instances have been thrown into this cruel world since the class itself should be held responsible for tracking its babies (using a class method and a class variable).

**Self**

Alright, finally coming back to this keyword... what is `self`? That sounds a lot like an question that rears its ugly head at 2 AM when you have to be up early the next morning. It is a keyword whose power seems almost immediately recognizable but ambiguous at the same time. `self`, like in real life, gives access to the object in the current scope. When I stand up and shout, "Self!", I might look strange, but I am referring to this body with my mind inside of it. I am the object and the receiver of the call, "Self!" `self` works similarly in Ruby. A class method is written as follows.

```
class Me

   @@all = []

   def self.all
     @@all
   end
	 
end
```

Within the class definition of Me above, a class method, `self.all`, is being defined within class scope. The self keyword here is referring to the class Me. Therefore, the `self.all` method is being defined as a class method that will be called on the Me class. In this case, `self.all` will be called from the terminal as

```
Me.all
```

and the Me class will read the data stored within the class variable, `@@all`, and return an array of all the instances of the class.

These concepts are the building blocks of building multiple objects that interact and collaborate. In my next post, I'll dive further into the relationships that can be build between class objects, and I'll also write about modules and how they can extend the functionality of classes and also remove repetitive code that is common among collaborating objects.

