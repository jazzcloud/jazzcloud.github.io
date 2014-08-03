---
layout: post
title:  "First refactoring step"
date:   2014-08-04 07:00:04
categories: refactoring rails
---

When I travel and renting a room on Airbnb, sometimes that room is far from the Wi-Fi router. I had to go across the room and look for the place where signal is not so bad.

And a program measuring quality of signal usually says "Extremely poor signal", and it simply means that internet will not work at all, but when it says "Very poor", you can be happy, because now you actually can do something.

Lets talk about how to turn extremely poor code of your rails application into very poor code :)

How to do that? Get your shit out of controllers! You controllers should be only responsible for requests, responses, sessions and cookies. But they usually contain lots of business logic.

What I suggest - is just one very small change. Create a folder named "Use cases", and put your business logic there.

Here's how it looks in an I application, I had to deal with:

[](use_cases_structure.png)

Every file in that folder is contains one class named by a verb, describing one small scenario, which lots of people in Agile community call "use cases":

Actual look of your use case will of course depend on your business logic, but one thing I can suggest, that every class should have "run" method.
So, controller actions start look like this after that change:

[]()

What did we achieved? Now we have all business logic related to selected entity, in one place.
And at some point you start realizing, that some of your model's methods are used only in a single use case. And this is a sign that you should move these methods to corresponding use case.
And this change means that your models became more clear.

So, that's pretty it. And I had to say, that it is really cheap change. We're not going hardcore with Robert Martin patterns of refactoring or super-strict Sandi Metz rules. We just made one obvious change.

After that change it is much easier to add new code and change old code related to that entity.


=================



