---
layout: post
title: "reader, writer, and accessor"
modified:
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2015-09-05T15:39:55-04:00
---

How does an `attr_reader` differ from an `attr_writer`, but more specifically... what do they do? I remember asking myself
this question when I was introduced to classes in Ruby. I'll be going over their exact functions and purposes in a second,
but here's a short explanation:

When I make a card transaction of let's say $1 (pretty sweet, I know), the merchant needs to **first** check if I have
sufficient funds in my bank account, and **second** subtract that dollar from my account and add it onto his account.

The first operation refers to a reader method. In almost every class, there many be some if not a bunch of information
we need to be able to 'read' in order to complete another operation. If our merchant is not able to read that I have enough
money in the bank, he won't be able to charge me. O' the sorrow!

The second operation refers to a writer method. Same with the reader, there are information pertaining to a particular class
that needs to be changed, or rerouted. In the case of our transaction, without being able to **rewrite** the money in my bank,
there's no way that he'd be able to take... O'.. the sorrow..

#####So how does this relate to Ruby..?

It turns out that, because these two types of methods are so frequently used---the developers created simpler ways to write them.
And this is where `attr_reader`, `attr_writer`, and `attr_accessor` come in.

To explain this better, let's create an empty `Person` class and initialize an instance of it.

{% highlight ruby %}
class Person
end

new_person = Person.new
{% endhighlight %}

###attr_writer
If I wanted to set `new_person`'s name to something, I might try `new_person.name = "The Terminator"`. But let's see what happens
when we try that.

{% highlight ruby %}
[3] pry(main)> new_person.name = "The Terminator"
NoMethodError: undefined method name=' for #<Person:0x007feb0c2a1468>
from (pry):4:in __pry__'
{% endhighlight %}

Okay, but why? I mentioned earlier that we needed to a way to access information from an object, and that the reader and writer methods
allow us to do just that.

Let's change our `Person` to carry some of that information. Here's the first way to make a writer method:
{% highlight ruby %}
class Person
  def name=(name)
    @name = name
  end
end
{% endhighlight %}

and here's Ruby's shortened 'magical' way of writing EXACTLY the former:

{% highlight ruby %}
class Person
  attr_writer :name
end
{% endhighlight %}

<span style="color: gray">*:If you're confused on what the '@' symbol is doing in front of name, it's basically a Ruby notation for an 'instance variable'which means that this variable exists in the scope of the whole object, not just within the writer method.*</span>

<span style="color: gray">*::In Ruby, parameters passed to methods don't have to be within a parentheses. With that said, our `def name=(name)` can be translated into `new_person.name=("The Terminator")` or `new_person.name = "The Terminator"`.*</span>

Awesome, now that we have that taken care of, let's go give this guy a name.

{% highlight ruby %}
[3] pry(main)> new_person.name = "The Terminator"
=> "The Terminator"
{% endhighlight %}

Cool. Neat. Sweet. Let's ask him what his name is.

{% highlight ruby %}
[4] pry(main)> new_person.name
NoMethodError: undefined method name' for #<Person:0x007f8e4330a2e0 @name="The Terminator">
from (pry):6:in __pry__'
{% endhighlight %}

I don't believe you. Ruby must be lying!

Not really. Sure we can give our `new_person` a name, but we still don't have anything set up to
retrieve his name. It just kind of floats out there for no damn reason. Let's go write that reader method.

###attr_reader
There are once again, two ways to write a reader method. Here's the first:

{% highlight ruby %}
class Person
  attr_writer :name

  def name
    @name
  end
end
{% endhighlight %}

And here's the shorter, sweeter way:
{% highlight ruby %}
class Person
  attr_writer :name
  attr_reader :name
end
{% endhighlight %}

You can see that calling `new_person.name` is reaching in for an 'instance variable' `@name`, and that's how other objects
'read' information from your `Person` class. Let's give it one last whirl.

We first have to give it writer and reader methods...
{% highlight ruby %}
class Person
  attr_accessor :name
end
{% endhighlight %}

What the hell is that, dude! You never told me about an `attr_accessor`! I know, I know, relax.

###attr_accessor

An `attr_accessor :name` is the same thing as writing `attr_reader :name` and `attr_writer :name`, it's another 'magical'
way of writing both of those in one line. It so happens that information we need to retrieve often gets manipulated, and vise
versa. Therefore, Ruby creates an easier and faster way to create both methods with a simple `attr_accessor`. Thanks Ruby!

Running our test on our new code gives us...

{% highlight ruby %}
[3] pry(main)> new_person.name = "The Terminator"
[4] pry(main)> new_person.name
=> "The Terminator"
{% endhighlight %}

Hurrah! It finally works! Now go and give our Terminator some extra features. Ayl bi bawk.
