---
layout: post
title:  "On refactoring"
date:   2014-08-03 13:32:00
categories: refactoring
---

I want to share an advise about refactoring, I learned recently:

__Don't push it too hard__ & __don't do it for free__

When you first open the world of refactoring for yourself, you tend to pick the first random thing in your project and refactor it to death.

By death I mean very abstract java-like code. At that point you might be very far from initial problem, your code has been solving.

And the same can happen if you find hard to explain to your customer he should pay for it, and decide to spend your own time on improving the situation.

The worst thing about it, that after a few weeks this refactored part of your application might go to trash, because customer realizes, there's already no need in this functionality.

Instead of this, just relax, and wait for new feature requests to come. And once it comes, take this part of your application, and think how you can make it uncoupled from the rest of application.

When you work with messy rails controllers, consider [extracting use cases from your controllers code](http://jazzcloud.co/refactoring/rails/dealing-with-extremely-poor-rails-code/).