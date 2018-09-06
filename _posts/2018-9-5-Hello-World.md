---
layout: post
title: 'Test post and first post :D'
published: true
---

Note: This is my first post. I won't be adding syntax highlighting or many images yet.

I wanted to start this blog because my friend was asking me how to solve coding problems. I figured that documenting my approach to a particular problem would be helpful to them (and myself since writing encourages me to develop a deeper understanding of what I'm doing).

The first problem I want to talk about is a simple one: What's the most efficient way to find all the factors of a number? For this problem, I'll use Python 3.0.

First, let's define a factor. A factor of a number `n` is `i` if `i` divides `n` without leaving a remainder. That is, if `n % i == 0` we say that `i` is a factor of `n`.


## Pseudocode

Let's get to drafting a solution. Having defined what a factor is, how do we obtain all of the factors for a number `n`? We can start with a simple example. Let `n = 24`. From the top of our heads, we know all the factors of `24` are: `1, 2, 3, 4, 6, 8, 12, 24`. We can confirm that this is the case by going through all the numbers between `1` and `24` and checking to see whether they divide `24` without leaving a remainder. But how do we do this for bigger numbers?

Programmatically, for an arbitrarily high `n`, we can check all the numbers in the range of `1` to `n` since any factor of `n` must be included in this range, i.e. a factor of `n` cannot be less than `1` and it cannot be greater than `n`. So, just to reiterate, here's what we have so far:

1. We want to find all of the factors for a number `n`.
2. By definition of a factor, any factor of `n` must be in the range of `1` to `n`.
3. To determine whether a number `i` in this range is a factor of `n`, we need to check if `n % i == 0`

A reasonable approach here, in coding terms now, would be to iterate through the range of `1` to `n`, i.e. `range(1, n + 1)` and check if `n % i == 0` for every number `i` in this range.
