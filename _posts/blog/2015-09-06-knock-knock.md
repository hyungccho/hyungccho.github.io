---
layout: post
title: "Knock, knock!"
modified:
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2015-09-06T15:39:55-04:00
---

Who's there?

...Blocks, and procs!

You might have already heard the saying that everything in Ruby is an object. Ah, yes.. The beauty of objects and our ability
to manipulate me fascinates me. Why are you looking at me like that? Are you trying to say I'm wrong? What? Not EVERYTHING in
Ruby is an object? LIAR!

#Blocks

That's what I thought when I heard blocks weren't objects. But it's true, blocks technically AREN'T objects in Ruby. Let's test this
theory in an example:

{% highlight ruby %}
{ puts "Hello, world!" } #=> SyntaxError
var = { puts "Hello, world!" } #=> SyntaxError
{% endhighlight %}

Without having a method calling on the block, it doesn't get executed.

Let's go over the basic syntax of block usage. You have probably seen blocks mostly in this form:

{% highlight ruby %}
array = [1, 2, 3, 4]

array.all? { |num| num < 5 }
#=> true

array.select do |num|
  new_num = num + 1
  puts new_num
end
#=> [2, 4]
{% endhighlight %}

As you can see here, the blocks are being called by the `all?` and `select` methods. Blocks come in two forms. The first one we see in our `all?` example shows the block in `{}` form. Blocks can take an argument, which is passed as `|num|` in our example, and it **executes the rest of the block for each element in the array**. The second form of a block is the 'do...end' block. It works in the same way as our curly-bracket enclosed block, accepting an argument, and **executing the code in the block once for each element of the array**. So why have two ways to write a block?

The answer is syntax. Typically, when we want execute a *multi-line* block, we want to use a 'do...end' block like we see with our select method. When we're executing *single* line blocks, we'll use a curly-bracket enclosed block.

Okay, well I get it. But if blocks aren't objects, they can't be utilized for anything else other than when methods call it. It seems useful sure, but what's the big deal? Why doesn't Ruby have blocks as objects?

Good question!

Blocks aren't objects, but Ruby has a sweet object called a **Proc** which is exactly what you were looking for! A block, that can be harnessed into a variable, and called upon when needed!

Here's how you'd create a new proc:

{% highlight ruby %}
prc = Proc.new { |element| puts element }
{% endhighlight %}

The proc, *is* the block. When we try to create a proc without a block, we get an error.

{% highlight ruby %}
prc = Proc.new
#=> ArgumentError: tried to create Proc object without a block
{% endhighlight %}

Now that we have a proc, how can we use this to our advantage?

{% highlight ruby %}
prc = Proc.new { |num| num - 5 }

def add_or_subtract(num1, num2, &prc)
  if prc.nil?
    return num1 + num2
  else
    prc.call(num1, num2)
  end
end
{% endhighlight %}

Let's take a look at this code. There's quite a lot of things going on here.

* **Line 1**: Nothing new, we're creating a `Proc` object and storing it in a variable called `prc`.
* **Line 3**: We've defined a method that takes `num1`, `num2`, and `&prc` as parameters. That's a new one; we'll go over it.
* **Lines 4-5**: If `prc` is `nil` then return the result of `num1` plus `num2`. Pretty simple.
* **Lines 6-7**: If `prc` is defined, then call the `call` method on `prc` and throw in `num1` and num2`. Okay, what does this `call` method do? Take a guess!
