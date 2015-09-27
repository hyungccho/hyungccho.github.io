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

**Note:** This post will use sorting algorithm examples from a previous post that I made.

#Time Complexity

What exactly is a time complexity, and why are people so obsessed with it? By definition, a time complexity measures the increase in an algorithm's operations **relative** to the growth of the size of the input. Don't worry about exactly what this means. We'll get to it soon.

As to why it's important, let's look back on our sorting algorithm example. By the time we had a million employees, an algorithm with a slow time complexity wouldn't cut it. It's applicable to many different real situations where data sets become large enough to impact the process time, and therefore the efficiency of your program as a whole. Using even one slow algorithm for a large company can break down their entire project, and that's why people are so obsessed in figuring out the runtimes of their programs.

There are three main notations used when computing time complexities: Big Omega, Big Theta, and Big O. Each notation focuses on a different aspect of time complexities, and some notations are more accurate in representing the efficiency of an algorithm than others.

**However,**

We will only focus on Big O in the post. It's the most relatable notation for our purposes, and the differences in the algorithms once you know one thoroughly is not difficult to pick up.

#Big O

![Graph of Big O Runtimes](https://s3.amazonaws.com/grapher/exports/sr4wt8ve1f.png)

<a title="View with the Desmos Graphing Calculator" href="https://www.desmos.com/calculator/zlo12jrc0w">  <img src="https://s3.amazonaws.com/calc_thumbs/production/zlo12jrc0w.png" width="200px" height="200px"     style="border:1px solid #ccc; border-radius:5px"  /></a>
