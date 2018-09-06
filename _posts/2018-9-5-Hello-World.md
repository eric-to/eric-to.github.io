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

## First Solution

A reasonable approach here, in coding terms now, would be to iterate through the range of `1` to `n`, i.e. `range(1, n + 1)` and check if `n % i == 0` for every number `i` in this range. Let's write this out:

``` python
# Given a number n, factors(n) will return a list containing all of the factors of n
def getFactors(n):
    factors = []
    for i in range(1, n + 1): # we use n + 1 because range() does not include the upper bound
    	if n % i == 0:
        	factors.append(i)
    return factors
```

Great! We have a working solution. But how fast is it? For most inputs we might care about: fast enough. On my macOS terminal, `getFactors(1000000)` finishes almost instantaneously. `getFactors(10000000)` runs slower and `getFactors(100000000)` substantially slower. You're probably wondering why we care about finding all of the factors for such large values of `n` anyways. Probably no one cares (half-joking). But in the CS world, there's a healthy obsession over improving an algorithm's **runtime**. If you're writing an algorithm that's meant to be used intensively / with large input, optimized runtime can make a world of difference. If the definition of runtime isn't self-evident, it means the length of time a program takes to run. In the case of `getFactors(n)` above, the runtime can be denoted by *O(N)*, *N* being the number whose factors we want to find.

This means that the runtime (may it be milliseconds or seconds), grows linearly, i.e. there is a one-to-one correspondence between the time it takes for the program to run and the size of the input. How do we know this? The majority of the work that happens in `getFactors(n)` happens in the `for` loop. The `for` loop runs `n` times or checks `n` number of values (`1` to `n` inclusive) to find all of the factors. But do we really have to check `n` number of values?

Not quite. Consider an input `n`. Is `n - 1` a factor of `n`? No. Is `n - 2` a factor of `n`? No. You may now realize that for any number `n`, its largest factor is no more than `n / 2`. Any factor greater than `n / 2` cannot divide `n` without leaving a remainder. So, one optimization we can make is changing the `for` loop to iterate only `n / 2` times, i.e. we only need to check `n / 2` numbers (`1` to `n / 2` inclusive). But for large inputs, this improvement is negligible. Think back to your early calculus class. As `n` approaches infinity, `n / 2` basically looks like `n`. In runtime analysis, constants are usually ignored.

## Optimizing

