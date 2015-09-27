---
layout: post
title: Big O Notation
modified:
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2015-09-27T15:39:55-04:00
---

#Time Complexity

What exactly is a time complexity, and why are people so obsessed with it? By definition, a time complexity measures the increase in an algorithm's operations **relative** to the growth of the input size. Don't worry about exactly what this means. We'll get to it soon.

As to why it's important, let's look back on our sorting algorithm example. By the time we had a million employees, an algorithm with a slow time complexity wouldn't cut it. It's applicable to many different real situations where data sets become large enough to impact the process time, and therefore the efficiency of your program as a whole. Using even one slow algorithm for a large company can break down their entire project, and that's why people are so obsessed in figuring out the runtimes of their programs.

There are three main notations used when computing time complexities: Big Omega, Big Theta, and Big O. Each notation focuses on a different aspect of time complexities, and some notations are more accurate in representing the efficiency of an algorithm than others.

**However,**

We will only focus on Big O in the post. It's the most relatable notation for our purposes, and the differences in the algorithms once you know one thoroughly is not difficult to pick up.

#Big O

This specific notation focuses on the worst case scenario of an algorithm. The question asked is: if our algorithm ended on its last iteration, what would its time complexity be?

There are seven main functions when describing Big O:

* Constant Time: O(1)
* Logarithmic Time: O(log n)
* Linear Time: O(n)
* Linearithmic Time: O(n log n)
* Quadratic Time: O(n<sup>2</sup>)
* Exponential Time: O(2<sup>n</sup>)
* Factorial Time: O(n!)

Let's go over some examples.

####Constant Time

{% highlight ruby %}
def output_first_element(arr)
  puts arr.first
end
{% endhighlight %}

In this example, no matter how small or big our `arr` is, our function would perform the same number of operations.

####Linear Time

{% highlight ruby %}
def output_every_element(arr)
  arr.each do |el|
    puts el
  end
end
{% endhighlight %}

You can see that in linear time, the amount of operations grows *linearly* relative to the number of elements. If there are a thousand elements in the array, there will be a thousand operations.

####Quadratic Time

{% highlight ruby %}
def output_every_pair_of_elements(arr)
  arr.each do |first_element|
    arr.each do |second_element|
      puts "#{first_element}#{second_element}"
    end
  end
end
{% endhighlight %}

This is O(n<sup>2</sup>) time. If we have 2 elements in `arr`, we will have to print 4 times. If we have 10 elements, 100 times. 100 elements? 10000 times. You can see how this gets out of control pretty quickly.

![Graph of Big O Runtimes](https://s3.amazonaws.com/grapher/exports/sr4wt8ve1f.png)

As you can see, our x-axis represents the number of elements in our data set, and our y-axis represents the number of operations performed to handle a given set. Can you guess which line represents which?

##Big O is represented by its significant term

We are not concerned with the rest of the algorithm's equation. To show what I mean, let's take our quadratic time method with a slight twist.

{% highlight ruby %}
def output_every_pair_of_elements(arr)
  arr.each do |first_element|
    puts "#{first_element}'s pairs:"

    arr.each do |second_element|
      puts "#{first_element}#{second_element}"
    end
  end
end
{% endhighlight %}

`puts` is our sole operation in this method, but now we're printing the first element once in addition to all of its pairs. Now the function to represent this algorithm SHOULD be n<sup>2</sup> + n, but it's still represented as O(n<sup>2</sup>). Why is this? Let's take a look at how this additional O(n) affects our algorithm when it gets huge.

Where `n = 10000`,
O(n<sup>2</sup>): 100,000,000 operations
O(n<sup>2</sup> + n): 100,010,000 operations

See how the extra 10,000 operations compared to the 100,000,000 isn't a big deal? That's why to keep things simple, Big O is only represented by its *slowest* function.

##Beware of Ignoring Constants!

But didn't you just say to represent an algorithm by its stripped slowest function? Yes, I did, but let's look at another example.

{% highlight ruby %}
def output_first_element(arr)
  puts arr.first
end
{% endhighlight %}

Here is our constant time example, but rather than printing it just once, I want to print it 5 times.

{% highlight ruby %}
def output_first_element(arr)
  5.times do
    puts arr.first
  end
end
{% endhighlight %}

Now our constant time function should be represented by O(5) but Big O treats it the same as O(1), and in this particular case it might not matter too much, but what if one operation in a different situation took a significant amount of time?

To imitate this, let's add a sleep to both of our methods.

{% highlight ruby %}
def output_first_element(arr)
  sleep 5
  arr.first
end
{% endhighlight %}

{% highlight ruby %}
def output_first_element(arr)
  5.times do
    sleep 5
    arr.first
  end
end
{% endhighlight %}

The first method would take around 5 seconds, and the second method 25 seconds, but both in Big O notation would be represented by O(1). You can see that in some situations, speeding up your algorithm by a factor of 5 can make a huge difference.

#Overview

So what have we learned? Big O calculates the worst case scenario of an algorithm. It doesn't care about constants or less significant terms, but that doesn't mean that we as aware programmers can forget about them. Understanding the time complexity of a given algorithm can save you from many problems down the line, and now that I know which algorithms are relatively quick, I can apply it to my sorting algorithms to give out bonuses!
