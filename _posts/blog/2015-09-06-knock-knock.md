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
to manipulate them fascinates me. Why are you looking at me like that? Are you trying to say I'm wrong? What? Not EVERYTHING in
Ruby is an object? LIAR!

#Blocks

That's what I thought when I heard blocks weren't objects. But it's true, blocks technically AREN'T objects in Ruby. Let's test this
theory in an example:

{% highlight ruby %}
{ puts "Hello, world!" } #=> SyntaxError
var = { puts "Hello, world!" } #=> SyntaxError
{% endhighlight %}

Without having a method calling on the block, it doesn't get executed.

Let's go over the basic syntax of block usage. You have probably seen blocks used mostly in this form:

{% highlight ruby %}
array = [1, 2, 3, 4]

array.all? { |num| num < 5 }
#=> true

array.each do |num|
  new_num = num + 1
  puts new_num
end
#=> 2, 3, 4, 5
{% endhighlight %}

As you can see here, the blocks are being called by the `all?` and `each` methods. Blocks come in two forms. The first one we see in our `all?` example shows the block in `{}` form. Blocks can take an argument, which is passed as `|num|` in our example, and it **executes the rest of the block for each element in the array**. The second form of a block is the `do...end` block. It works in the same way as our curly-bracket enclosed block, accepting an argument, and **executing the code in the block once for each element of the array**. So why have two ways to write a block?

The answer is syntax. Typically, when we want execute a *multi-line* block, we want to use a `do...end` block like we see with our each method. When we're executing *single* line blocks, we'll use a curly-bracket enclosed block.

Okay, well I get it. But if blocks aren't objects, they can't be utilized for anything else other than when methods call it. It seems useful sure, but what's the big deal? Why doesn't Ruby have blocks as objects?

Good question!

#Proc

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

def add_or_subtract(num1, num2, prc)
  if prc.nil?
    num1 + num2
  else
    prc.call(num1, num2)
  end
end
{% endhighlight %}

Let's take a look at this code. There's quite a lot of things going on here.

* **Line 1**: Nothing new, we're creating a `Proc` object and storing it in a variable called `prc`.
* **Line 3**: We've defined a method that takes `num1`, `num2`, and `prc` as parameters. That's sort of new, we'll go over it.
* **Lines 4-5**: If `prc` is `nil` then return the result of `num1` plus `num2`. Pretty simple.
* **Lines 6-7**: If `prc` is defined, then call the `call` method on `prc` and throw in `num1` and `num2`. Try taking a guess to what this does!

Let's begin to sort this out case by case. The first case we'll go over is I think the easiest to understand. The case where we pass in all variables like a good little programmer.

{% highlight ruby %}
add_or_subtract(5, 4, prc)
#=> 0
{% endhighlight %}

The proc we passed in as `prc` is not `nil`, therefore, it executes line 7 by using the proc's `call` method. This method is straight forward in that all it's doing is passing into the proc the arguments supplied in as parentheses. So essentially, our `prc.call(num1, num2)` method looks like this in a block. `{ |num1, num2| num1 - 5 }`.

Now you might have noticed that we're accepting one argument in our `prc` variable, but we're passing two parameters to it during the call. Good catch! However, that's a neat feature of procs. They don't care that we're throwing in the wrong number of variables. It just executes the call using the first `n` number of parameters it needs to execute it. The proc call won't work if you don't supply enough arguments.

Let's move on, but this time, let's add one new character in our `add_or_subtract` definition.

{% highlight ruby %}
prc = Proc.new { |num| num - 5 }

def add_or_subtract(num1, num2, &prc)
  if prc.nil?
    num1 + num2
  else
    prc.call(num1, num2)
  end
end
{% endhighlight %}

<span style="color: gray">*:If you don't see it, shame on you! It's in front of the `prc` on line 3. The squiggly character called an ampersand.*</span>

What this does is it calls the `to_proc` method on the variable. Our previous example worked, but what if we don't always have a proc object to pass in as an argument? Or what if we wanted to do something other than subtract the first number by 5? We would have to create yet another proc object and then pass that in as an argument. There must be a handier way to do it. And there is! It's the ampersand symbol that allows us to do just that. So let's say we don't have an explicit proc object to pass into the method, but we do know what we want to do inside the block. We can write it like this:

{% highlight ruby %}
add_or_subtract(5, 4) { |num| num - 2 }
#=> 3
{% endhighlight %}

The ampersand makes passing the proc in as a variable optional, and looks for a block following the method call. If there is, then it stores the block into `prc` so that we can use it for something else if we wanted to. If there isn't, then

Similarly, we might never create a proc object before calling our method, and sometimes we want the prc to default into something if it doesn't have a block following it. Here's an example:

{% highlight ruby %}
array = [2, 4, 5, 1, 3]

array.sort { |num1, num2| num2 <=> num1 }
#=> [5, 4, 3, 2, 1]

array.sort
#=> [1, 2, 3, 4, 5]
{% endhighlight %}

Here's what's kind of going on behind the scenes:
<span style="color: gray">*:This implementation of sort below is bubble sort. Ruby's built in sort method doesn't use bubble sort, but for demonstration purposes, we'll be using bubble sort*</span>

{% highlight ruby %}
class Array
  def sort(&prc)
    prc ||= Proc.new { |num1, num2| num1 <=> num2 }

    sorted = false
    until sorted
      sorted = true

      (self.length - 1).times do |i|
        if prc.call(self[i], self[i + 1]) == 1
          self[i + 1], self[i] = self[i], self[i + 1]
          sorted = false
        end
      end
    end
    self
  end
end
{% endhighlight %}

Don't worry if you don't know what that does. The only thing you should take from our sort example is that it takes an `&prc` parameter just like our method, and it uses the passed proc if it's not `nil`. If it is `nil`, we set it to our own default proc. Because we have it set up like this, we don't ever have to explicitly create a proc object before calling the method. Pretty neat.

You might have also noticed that we never `return` inside a block. That's because when we do something like

{% highlight ruby %}
array = [2, 4, 5, 1, 3]

array.sort { |num1, num2| return num1 <=> num2 }
#=> LocalJumpError: unexpected return
{% endhighlight %}

Ruby doesn't expect us to `return` inside a block. The return value is implicit, meaning `array.sort { |num1, num2| num1 <=> num2 }` will implicitly return -1, 0, or 1.

<span style="color: gray">*:There is a way to explicitly return from a proc using something called a lambda. However, I wont be going over lambdas in this post.*</span>

Now that we've come this far, let's go over some of the properties of procs and blocks.

* Procs are objects in Ruby, blocks are not.
* Procs can be passed in as an optional argument using `&`. It defaults to a block following the call on the method if it's not passed in explicitly, or `nil` if neither are supplied.
* Procs can take any number of arguments so long as it supplies **enough** arguments.

I would say knowing procs and blocks cover 90% of all usage in basic Ruby, but there is one other form of proc, and one other way to access a block. The other form of a proc is called a lambda, and the other way to access a block is through `yield`. I'll only be going over `yield` in this post.

#Yield

We know by now what a block looks like, how it takes parameters, and how it returns implicitly based on executing the code in the block. We also know how to set that block into a proc and access it using a proc, but there is another way using `yield`.

Let's go ahead and rewrite our initial `add_or_subtract` method using `yield`.

{% highlight ruby %}
def add_or_subtract(num1, num2)
  if block_given?
    yield(num1, num2)
  else
    num1 + num2
  end
end
{% endhighlight %}

Looks similar, but instead of using `prc.nil?` we use `block_given?`, and instead of using `prc.call(num1, num2)` we use `yield(num1, num2)`. If it looks like a duck, swims like a duck, and quacks like a duck, it probably is a duck, right? Not exactly. If we look back to the example where we used `&prc`, Ruby didn't care whether or not we assigned a block to it. But `yield` is like a sensitive teen. Here it is in action, but we'll modify our method a little to try and mimic the `&prc`:

{% highlight ruby %}
def add_or_subtract(num1, num2)
  if yield.nil?
    num1 + num2
  else
    yield(num1, num2)
  end
end

add_or_subtract(5, 3)
#=> LocalJumpError: no block given (yield)
{% endhighlight %}

Okay, fine have it your way. I'll give you a damn block.

{% highlight ruby %}
add_or_subtract(5, 3) { |num| num - 5 }
#=> NoMethodError: undefined method '-' for nil:NilClass
{% endhighlight %}

GRRRRR... FINE.
{% highlight ruby %}
def add_or_subtract(num1, num2)
  if block_given?
    yield(num1, num2)
  end
end

add_or_subtract(5, 3) { |num| num - 5 }
#=> 0
{% endhighlight %}

You can see through this that `yield` needs to meet pretty specific conditions, and if it doesn't will throw errors. One advantage to yield, however, is that it processes the block (almost 2x) faster than a proc does. Which I guess is cool...

So there you have it! If I left you hanging with questions, feel free to contact me. I'm available by e-mail: hyungchulcho93@gmail.com. Ciao!
