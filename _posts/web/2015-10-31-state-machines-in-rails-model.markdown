---
layout: post
title: "State Machine in Rails Model"
modified:
categories: web
excerpt: "Learn how to implement state machine in your Rails app."
tags: ["rails","state machine","model","machine"]
image:
  thumb: aasm_flow.png
comments: true
date: 2015-10-31T16:01:33+05:30
---

Last time I heard 'state machine' was 5 years back when I was dozing in my computer science lecture. Professor drew some flowchart with math notations on board to explain us the concept of computation around finite states.

State machine aka finite state machine is a system consisting of finite number of states, rules to map one state to another or itself for any possible inputs. The system is in one state at a time and can transition to other allowed state by triggering an event.

As a rails developer, you won't easily encounter state machine until you find it being used in someone else's code or some gem. Few weeks back, I was reading through 'Mastering Modern Payments' to integrate Stripe in my Rails app and I found the concept being used there. Its one of the best things we could use to design a model having multiple states. We won't be diving into what is being abstracted away and focus more on 'when to use them and how can we use them in our rails app'.

Every modern application feature is built around a flow of events. For instance, if we consider 'ordering a product' on an ecommerce application, the most common and minimal flow of events would be as follows.

- User places an order.
- Warehouse team processes the order.
- Packing team packs the orderered products.
- Shipping team ships the order to nearest hub.
- Delivery guys deliver the ordered products to user.
- At any point of time before delivery, user can cancel the order.

Representing above flow in a diagram
<figure>
  <img width="500px" src="/images/aasm_flow.png">
</figure>

At any given time, our entity 'order' will be under one state and can transition to other permissible states on triggering required event.

###So why should I use state machine to design this in Rails?

I heard you. <br>
Ofcourse, you can design above model without using state machine. You could just write some validations to make it work. Its ok to do that as long as your model is limited to a couple of states. But imagine writing the same for a model that has 5-6 different states like our Order model. You would end up beefing up your model with 100 lines of validations just to check the validity of state transitions. If you need to add one more state in your model later, you would again need to go through all the code to check if adding a state could pop any issues. Ultimately, this approach becomes cumbersome to manage and is more prone to bugs.

The main reason for using state machine is to help the design process. It is much easier to figure out all the possible edge conditions by drawing out the state machine on paper. This will make sure that your application will have less bugs and less undefined behavior.

##Using state machine in ActiveRecord model.

We will be using this awesome gem called [AASM](https://github.com/aasm/aasm){:target="_blank"} (Act As State Machine). Its a generic library that provides adapters to support various models. We will just be dealing with ActiveRecord model in this post.

###Usage

Install the gem as instructed under documentation.
Lets design our Order model.
{% highlight ruby %}
class Order < ActiveRecord::Base
  include AASM

  aasm column: 'state' do
    state :placed, :initial => true
    state :processing
    state :packed
    state :shipped
    state :delivered
    state :cancelled

    event :process do
      transitions :from => :placed, :to => :processing
    end

    event :pack do
      transitions :from => :processing, :to => :packed
    end

    event :ship do
      transitions :from => :packed, :to => :shipped
    end

    event :deliver do
      transitions :from => :shipped, :to => :delivered
    end

    event :cancel do
      transitions :from => [:placed, :processed, :packed, :shipped], :to => :cancelled
    end
  end

end
{% endhighlight %}

Whoa! There we go. By just looking at this code, we can picture the flow of our model. It is self explanatory. We have defined our states, events and transitions as per our design. Now lets explore the awesomeness this gem provides out of the box.

It provides us with nifty instance methods.
{% highlight ruby%}
o = Order.new  # new order obj with status as 'placed'
o.placed?      # returns true
o.processing?  # returns false
o.may_process? # returns truthy value
o.may_cancel?  # returns truthy value
o.may_deliver? # returns false

o.process   # transitions state to 'processing' but does not save it
o.process!  # transitions state to 'processing' and saves it

o.deliver # raises AASM::InvalidTransition: Event 'deliver' cannot transition from 'processing'
{% endhighlight %}

Ain't you already drooling?
<figure>
  <img width="500px" src="/images/homerdrool.gif">
</figure>

If you dont want to raise exceptions, you could do
{%highlight ruby%}
aasm :whiny_transitions => false do
  #...
end
{% endhighlight %}

and it will return `false` instead of raising exception.

We can also pass a block to state changing methods (events). Block will only execute if transition succeeds.
{%highlight ruby%}
o.ship! do
  o.user.order_shipped_email
end
{% endhighlight %}

We can also define callbacks around states, events and transitions. They will be called when certain criterias are met (eg leaving/entering a state)

{%highlight ruby%}
state :processing, :before_enter => :do_blabla

event :deliver do
  after do
    # called after transition 'shipped' to 'delivered' is finished
  end
  transitions :from => :shipped, :to => :delivered
end
{% endhighlight %}

You can check the available callbacks and their order of execution documented [here](https://github.com/aasm/aasm#callbacks){:target="_blank"}.

We can also use guards (:guard, :if, :unless) to transition states conditionally.
{%highlight ruby%}
event :cancel do
  transitions :from => [:placed, :processed, :packed, :shipped], :to => :cancelled, :if => :can_cancel_order?
end
{% endhighlight %}

AASM also provides us with scope methods to query required states
{%highlight ruby%}
Order.shipped # returns all records with state as shipped
Order.placed.where('created_at >=  ?', 30.days.ago)
{% endhighlight %}

If you are using Rails version 4.1+, chances are high that you are using ActiveRecord Enums.

Good news is AASM plays well with Enums.
{%highlight ruby%}
enum state:{
  placed: 0,
  processing: 1,
  packed: 2,
  shipped: 3,
  delivered: 4,
  cancelled: 5
}

aasm column: 'state', enum:true do
  ...
end
{% endhighlight %}

Lets take a look back to check what all did 'AASM' do for us

<i class="fa fa-check-square-o"></i> Set default value for our state field<br>
<i class="fa fa-check-square-o"></i> Validate transitions from one state to another<br>
<i class="fa fa-check-square-o"></i> Callbacks to invoke function on state transition<br>
<i class="fa fa-check-square-o"></i> Instance methods to get/set state with other useful helpers<br>
<i class="fa fa-check-square-o"></i> Scope methods to query db for required states

Imagine having to do all these without using state machine.
<figure>
  <img width="500px" src="/images/nosmith.gif">
</figure>

There are many other things under AASM that we can customize and extend it as required. Refer gem documentation for more details.

So that’s a taste of state machine. Hope you're taking away something good from this post.

Cheers!