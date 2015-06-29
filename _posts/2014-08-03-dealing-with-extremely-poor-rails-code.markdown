---
layout: post
title:  "Dealing with extremely poor rails code"
date:   2014-08-03 07:00:04
categories: refactoring rails
comments: true
---


_When I travel and renting a room on Airbnb, sometimes that room is far from Wi-Fi router. I had to go across the room and look for place where signal is not so bad._

_And when a program measuring quality of signal usually says "Extremely poor signal", it means that internet will not work at all, but when it says "Very poor", I am happy, because now I can actually do something._

Lets talk about how to turn extremely poor code of your rails application into very poor code :)

If you're sure you not [pushing it too hard](/refactoring/on-refactoring), it is time to __get shit out of your controllers!__

Controllers should be only responsible for requests, responses, sessions and cookies. But they usualy contain lots of business logic.

Make just one very small change - create a folder named "use_cases" and put your business logic there.

Here's how it looks in an application, I had to deal with:

<center><img src='/images/use_cases_structure.png'/></center>

Every file in that folder is contains one class named by a verb, describing one small scenario, which lots of people in Agile community call "use case":

{% highlight ruby %}
module UseCases::Samples
  class UseCase
    def initialize(initiator, project_id)
      @initiator = initiator
      @project_id = project_id
    end

    #All your hairy stuff here:
    def run(input_data)
      output = []
      some_really_complex_stuff_here(input_data) do |data|
        b = project.bla(data)
        c = initiator.blabla(project_id, data, b)
        output << bla!(initiator, project_id, c)
      end
      output
    end

    private
    attr_accessor :initiator, :project_id

    def project
      @project ||= Project.find_by_id(project_id)
    end
  end
end
{% endhighlight %}

You can go as wild as you want inside your use cases - implement everything in bash scripts, pure SQL or whatever you want. The rest of the application doesn't care about it. It's ok, until use case returns correct data, which you views are waiting for.


Actual look of your use case will of course depend on your business logic, but one thing I can suggest, that every class should have "run" method.
So, controller actions start look like this after that change:

{% highlight ruby %}
def create
  use_case = UseCases::Samples::Create.new(current_user, @project.id)
  @samples = use_case.run(params[:samples])
  render_results
end

def mass_update
  use_case = UseCases::Samples::Update.new(current_user, @project.id)
  use_case.run(params[:samples])
  render_results
end
{% endhighlight %}



This is very small, but very significant change. What we have now:

* Controllers don't know anything about your models. They only pass some params to one use case, and pass results to view.
* All business logic related to selected entity located in one place. It is much simpler now to go further with refactoring of each use case, because now its code is separated from other parts of application.
* At some point you start realizing, that some of your model's methods are used only in a single use case. This is a sign that you should move these methods to corresponding use case. And this leads to more clear models.


So, that's pretty it. I had to say, that it is really cheap change. We're not going hardcore with [Martin Fowler's patterns of refactoring](http://www.amazon.com/Refactoring-Edition-Addison-Wesley-Professional-Series/dp/0321984137) or super-strict [Sandi Metz rules](https://www.youtube.com/watch?v=npOGOmkxuio).

We just made one obvious change and got huge profit in pretty bad situation!

For further reading I recommend these articles:

* "[Writing Use Cases](http://hawkins.io/2014/01/writing_use_cases/)" by [Adam Hawkins](http://twitter.com/admin65)
* "[Services - what are they and why we need them](http://blog.arkency.com/2013/09/services-what-they-are-and-why-we-need-them/)" by [Marcin Grzywaczewski](http://twitter.com/killavus)
