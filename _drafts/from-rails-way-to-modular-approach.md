---
layout: post
title:  "From rails way to modular architecture."
date:   2014-08-03 17:17:24
categories: rails architecture
---
I used to be regular Rails developer. It was hard for me to imagine how to develop something without Rails.
But in a lots of applications(both mine and other teams’) I noticed same pattern:
1. Your app is clean and shiny. You deliver new features to a customer with speed of light.
2. You start using some gems, cause you need more functionality. Some of the gems don’t fit ideally. Some of functionality requires a bit of hacking, because Rails-way doesn’t seem to help all the time.
3. Your customer expects that you still can work with same speed, but you already can’t. Because you’ve added so much stuff, that  it slows you down
4. If you keep adding stuff, at one point you find yourself in the situation that any step forward is very painful. You changed small thing here, and something breaks in the other part.

I am sure, every Rails developer have wrote or seen the code like this. This didn’t seem right to me. Of course writing complex web-applications can be challenging, but resistance shouldn’t be that hard.

So I started to google hard. And I found that there’s tons of principles, patterns and other stuff to manage complexity, so that your code will not bury you.
I started to read books and articles, watch videos from conferences about it. And I often was internally agree what they’ve been talking, but I had no idea how to apply those principles in my code.
When I looked at some good code, I could say that it is good. But I was not able to write code like this.

So knowing about something is not the same as knowing how to apply it.

But if you want something really hard, life will give you a chance.

And that chance was a new customer. 

# The story

Customer had Grails application exactly like on stage 4 from the list above. Current team stuck. Everything become too fragile, and new feature could have been developed for weeks.
\_ And customer had a solution. His idea was to implement backend using Rails from scratch. And there was second team, which worked on front-end stuff using Angular.

That sounded like a good plan, so I accepted the offer.
The only limitation was that I can not touch database structure. New application should work in parallel with old one

Ok, let’s do it.

# Models
And like every Rails developer I started from models.

Database was not following Rails conventions, so I had to do some additional configuration in models in order to work with existing tables:

\`\`\`ruby
\`class Profile \< ActiveRecord::Base
  self.table\_name = 'profile'
  belongs\_to :image, foreign\_key: :picture\_id
end

class Image \< ActiveRecord::Base
  self.table\_name = 'image'
  has\_and\_belongs\_to\_many :image\_variants,
  join\_table: "image\_image", class\_name: "Image",
  association\_foreign\_key: :image\_images\_id
  belongs\_to :settings,
  foreign\_key: :settings\_id, class\_name: 'ImageSettings'
  belongs\_to :asset
end
\`\`\`
\`

Once it was done, I had access to the data, and ready able to do something useful.
And I decided to use RABL to hide unneeded data from users:

\`\``ruby`
collection @object
attribute :id, :deleted, :username, :age

node :gender do |object|
  object.gender.to\_s
end

node :thumbnail\_image\_url do |obj|
  obj.thumbnail\_image.asset.url
end

node :standard\_image\_url do |obj|
  obj.standard\_image.asset.url
end
\`\`\`
\`
So, I was ready to implement first feature. 

# Users Registration

Surprising thing(or not so surprising) was that system had three different kinds of users. And every kind of user had each own business logic for registration.

![][image-1]

`Admin` had to fill complex form with many required fields. Organization user had different form with another set of validations. And regular user had the simplest form

And of course they were stored in the same table called `users`.

What is the default Rails way to solve this kind of problems? Of course, we have conditional validations.

![][image-2]

But for some reason nobody likes them. With conditional validations you mix too many stuff in one model.

Let’s take a look what are those things:
- how data is stored
- how ‘Admin’ form should be validated
- how ‘Organization user’ form should be validated
- how ‘Regular user’ form should be validated

We can even call those things “responsibilities”. And remember that there is a single responsibility principle.

If we try to separate these responsibilities, it is impossible not to remember about Form Objects. It is obvious that validation stuff should belong to forms, not to model:


\`\``ruby`
class OrgUserInput
  include Virtus.model
  include ActiveModel::Validations

  attribute :login, String
  attribute :password, String
  attribute :password\_confirmation, String
  attribute :organization\_id, Integer

  validates\_presence\_of :login, :password, :password\_confirmation
  validates\_numericality\_of :organization\_id
end

input = OrgUserInput.new(params)
if input.valid?
  @user = User.create(input.to\_hash)
else
# ...
end
\`\`\`
\`
User model has become just a DB table wrapper. It might scare you, but I removed all validations from ‘User’ model completely. 
I was so fearless because there were constraints in existing database, so data consistency problem was solved on that layer.

If you don’t have constraints in your DB, then maybe you should have them :) Or of course you can have validations in model. But it is useful to think about form validity and data consistency as a different things because they are different.

So with form objects, we now had 4 simple objects instead of one complex. The cons is that if you want to use nested forms, it might be not that easy.

Let’s implement another feature

# Bonuscode redeem

![][image-3]

It looks simple. One person gives some code. System adds credits to his account.
But we need to check few things first:
- if bonuscode exists
- if bonuscode not already used
- if recipient exists
- if recipient is right kind of users

Here’s how it can be implemented in a default Rails way: 

![][image-4]

But if you try to think about responsibilities, it is obvious that there’s business logic and content delivery here mixed in one controller method:

![][image-5]

For years this was a serious question for me. Where should I put the business logic? Into `bonuscode.redeem_by(user)` method or `user.redeem_bonus(code)`?

Usually I was just making random decision here. But this time I was already familiar with a thing called “Services layer”, so I know that business logic code should be extracted into service.

```ruby
`class RedeemBonuscode
  def run!(hashcode, recipient_id)
raise BonuscodeNotFound.new unless bonuscode = find_bonuscode(hashcode)
raise RecipientNotFound.new unless recipient = find_recipient(recipient_id)
raise BonuscodeIsAlreadyUsed.new if bonuscode.used?

ActiveRecord::Base.transaction do
  amount = bonuscode.redeem!(recipient_id)
  recipient.increase_balance!(amount)
  recipient.save! && bonuscode.save!
end
recipient.balance
  end
end
```
`
And here’s how you can use it from the controller:

```ruby
`def redeem
  use_case = RedeemBonuscode.new

  begin
recipient_balance = use_case.run!(params[:code](), params[:receptor_id]())
  rescue BonuscodeNotFound, BonuscodeIsAlreadyUsed, RecipientNotFound =\> ex
render json: {error: ex.message}, status: 404 and return
  rescue TransactionError =\> ex
render json: {error: ex.message}, status: 500 and return
  end

  render json: {balance: recipient_balance}
end
```
`
Again, we separated responsibilities. Business logic is not mixed with controller code. And it does not depend on ActionController. So it will be easier to modify your business logic, or to upgrade version of Rails.




[image-1]:	http://assets.jazzcloud.co.s3.amazonaws.com/railsway_to_modular/users-forms.png
[image-2]:	http://assets.jazzcloud.co.s3.amazonaws.com/railsway_to_modular/conditional-validations.png
[image-3]:	http://assets.jazzcloud.co.s3.amazonaws.com/railsway_to_modular/redeem.png
[image-4]:	http://assets.jazzcloud.co.s3.amazonaws.com/railsway_to_modular/controller_src.png
[image-5]:	http://assets.jazzcloud.co.s3.amazonaws.com/railsway_to_modular/controller.png