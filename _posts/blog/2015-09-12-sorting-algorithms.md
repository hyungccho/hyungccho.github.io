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

#Introduction

Being able to control the way data is sorted is a powerful tool for almost any situation. Let's say that I own a company of five people. Christmas is approaching and our sales have climbed 1000% in the last year (That would be quite nice...). And as a bonus for hard work, I want to give my five loyal employees a bonus depending on how long they've been with the company. Okay, sounds simple enough. If I wanted to do this, I can possibly look in my employee database, and **sort** them by the date they started working to determine their bonuses. But then again... It is only five people. I might just remember the general timeframe they came in. Fine. Let's fast forward five years...

...Now I have a hundred employees. See where this is going? There's no way I'll remember when everyone started working. Heck, I'm not sure I'll know everyone's name at this point. In this situation, having a nifty database of employees that I can sort would allow me to hand out bonuses accordingly come Christmas. Cool. But somewhere down the line...

...Maybe I have a million employees. That's right. A million. At this point, I probably know all of five people who work in the company, and if I'm still responsible for determining and handing out bonuses, I've done something wrong. However, let's say I'm still responsible, and I haven't updated my sort method to accompany this growth. I'm using some slow sort method that might've worked fine when I had a hundred employees, but now I have a million. It takes a whole day's worth of processing to sort properly. That is unacceptable. Bonuses must be given! Because of this, I'll need a much faster sorting algorithm in work to match my needs. So let's go over some of the basic and quite commonly used sorting algorithms.

We won't be focusing on time complexities in this post (We'll go over Big O and each sorting algorithm's respective time complexities in the next post), but rather just the underlying algorithms behind each sorting technique.

The four sorts we'll do are **insertion sort**, **bubble sort**, **merge sort**, and **quick sort**.

#Insertion Sort

The idea behind insertion sort is to go through each element starting from the second element (the element at index 1), and iteratively compare it to preceding elements of the array until one of two conditions are met. When the current element finds a value that's lower than it, or when it reaches the beginning of the data set, it **inserts** itself into the respective position and the same loop commences again for the next value in the data. Here it is in Ruby:

```rb
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
```

If you notice, the value at index `i` represents our `current_value`, and we iterate through all its preceding elements represented by a decrementing `j`.

#Bubble Sort

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
end
#Merge Sort
#Quick Sort
