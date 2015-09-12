---
layout: post
title: Sort War!
modified:
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2015-09-12T15:39:55-04:00
---

Being able to control the way data is sorted is a powerful tool for almost any situation. Let's say that I own a company of five people. Christmas is approaching and our sales have climbed 1000% in the last year (That would be quite nice...). And as a bonus for hard work, I want to give my five loyal employees a bonus depending on how long they've been with the company. Okay, sounds simple enough. If I wanted to do this, I can possibly look in my employee database, and **sort** them by the date they started working to determine their bonuses. But then again... It is only five people. I might just remember the general timeframe they came in. Fine. Let's fast forward five years...

...Now I have a hundred employees. See where this is going? There's no way I'll remember when everyone started working. Heck, I'm not sure I'll know everyone's name at this point. In this situation, having a nifty database of employees that I can sort would allow me to hand out bonuses accordingly come Christmas. Cool. But somewhere down the line...

...Maybe I have a million employees. That's right. A million. At this point, I probably know all of five people who work in the company, and if I'm still responsible for determining and handing out bonuses, I've done something wrong. However, let's say I'm still responsible, and I haven't updated my sort method to accompany this growth. I'm using some slow sort method that might've worked fine when I had a hundred employees, but now I have a million. It takes a whole day's worth of processing to sort properly. That is unacceptable. Bonuses must be given! Because of this, I'll need a much faster sorting algorithm in work to match my needs. So let's go over some of the basic and quite commonly used sorting algorithms.

We won't be focusing on time complexities in this post (We'll go over Big O and each sorting algorithm's respective time complexities in the next post), but rather just the underlying algorithms behind each sorting technique.

The four sorts we'll do are **bubble sort**, **insertion sort**, **merge sort**, and **quick sort**.

#Bubble Sort

Bubble sort, one of the most simple sorting algorithms, repeatedly loops through the array, comparing each pair of adjacent elements, and sorts them if they are not ordered. Here's the code:

{% highlight ruby %}
class Array
  def bubble_sort!
    sorted = false
    until sorted
      sorted = true

      (self.length - 1).times do |i|
        if self[i] > self[i + 1]
          self[i], self[i + 1] = self[i + 1], self[i]
          sorted = false
        end
      end
    end

    self
  end
end
{% endhighlight %}

Taking a look at our code shows us two loops. The outer loop `until sorted` and the inner loop `(self.length - 1).times`.

What we're doing with the first one is saying "Hey Ruby, until you're told that the array is sorted, keep looping through". This trigger comes in the form of a boolean variable `sorted`. The inner loop executes itself `self.length - 1` times, and if any pair was not sorted during this run through the array, we change the `sorted` variable back to `false`.

If you're a visual learner, this resource will be amazing: [Bubble Sort](http://www.algomation.com/player?algorithm=541a6ea7a7fe980200089c5e)

#Insertion Sort

The idea behind insertion sort is to go through each element starting from the second element (the element at index 1), and iteratively compare it to preceding elements of the array until one of two conditions are met. When the current element finds a value that's lower than it, or when it reaches the beginning of the array, it **inserts** itself into the respective position and the same loop commences again for the element at index + 1. Here it is in Ruby:

{% highlight ruby %}
class Array
  def insertion_sort!
    (1..self.length - 1).each do |i|
      current_value = self[i]

      j = i - 1
      while j >= 0 && self[j] > current_value
        self[j + 1] = self[j]
        j -= 1
      end

      self[j + 1] = current_value
    end
    self
  end
end
{% endhighlight %}

You might have already noticed that the value at index `i` represents our `current_value`, and we iterate through all its preceding elements represented by a decrementing `j`.

Once again, algomation in action: [Insertion Sort](http://www.algomation.com/player?algorithm=5414f43062a6d502003341bc)

#Merge Sort

Merge sort is another sorting algorithm that involves recursion. It takes an array, and constantly splits it in half until each half has only element in it. Once you have it dwindled down to arrays of one element, it makes its way back up to the full array by merging them back in the correct order.

{% highlight ruby %}
class Array
  def merge_sort!
    return self if self.length < 2

    mid = self.length / 2
    left = self[0...mid]
    right = self[mid..-1]

    merge!(left.merge_sort!, right.merge_sort!)
  end

  def merge!(arr1, arr2)
    result = []

    until arr1.empty? || arr2.empty?
      if arr1.first < arr2.first
        result << arr1.shift
      elsif arr1.first > arr2.first
        result << arr2.shift
      else
        result << arr1.shift
        result << arr2.shift
      end
    end

    result.concat(arr1).concat(arr2)
  end
end
{% endhighlight %}

You can see we have two methods that combine to do the work of `merge_sort!`. Starting from the top, we split the array into two halves, expressed by:

{% highlight ruby %}
mid = self.length / 2
left = self[0...mid]
right = self[mid..-1]
{% endhighlight %}

We then call our `merge_sort!` recursively onto each half of our array, and finish it off by calling `merge!` on these two arrays.

Let's take a look at our helper method: `merge!`.

`merge!` takes in two arrays as arguments (which will be our halves) and sorts them by `shift`ing off the lower of the two first elements of the array. We concatenate the three arrays which ensures that our array will be sorted into one array, and return it.

This visual example might help understand it: [Merge Sort](http://www.algomation.com/player?algorithm=551321f6e1b6fa0300aae4d0)

You can see from this example that no sorting is done until every array is of length 1. This is because we're calling our `merge_sort!` recursively with the base case being `return self if self.length < 2`.

#Quick Sort

Quick sort is similar to merge sort in terms of performance, with a slight edge given with good implementation. This is because of the pivot implementation for this algorithm. Here's how it works.

`quick_sort!` picks a pivot point (in this case the first value of the array for simplicity), and drops out of the array. We loop through the rest of the array inserting the elements into two arrays based on whether the value is greater or lower than our `pivot`. When we have these two arrays populated, we call `quick_sort!` on both the arrays recursively, which then repeats these steps.

Once the arrays are split up into arrays consisting of only one element, all the greater and lower arrays add back up to our full array.

{% highlight ruby %}
class Array
  def quick_sort!
    return self if self.length < 2

    pivot = self.take(1)
    lower = []
    greater = []

    self.drop(1).each do |el|
      if el < pivot.first
        lower << el
      else
        greater << el
      end
    end

    lower.quick_sort! + pivot + greater.quick_sort!
  end
end
{% endhighlight %}

In case you're unsure what our `take(1)` and `drop(1)` is doing:
* [Array#take](http://ruby-doc.org/core-2.2.0/Array.html#method-i-take)
* [Array#drop](http://ruby-doc.org/core-2.2.0/Array.html#method-i-drop)

There are a few different ways quick sort is implemented. You can use a quite inefficient `pivot` as I did, or you can set it to the median of the array, or just the middle value of the array. I purposely did not put up a visualization for quick sort, because I couldn't find anything that clearly shows what's going on.

####There it is!

There are many different sorting algorithms out there, but these are the four that I've decided to go over. I'll be going over Big O, and then each algorithm's time complexities in the future. These algorithms are *sorted* based on speed and efficiency. Can you guess which end is which?

Once again, if you have any questions or comments about anything code related I'll be happy to chat!
