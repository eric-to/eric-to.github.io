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

It might not seem obvious at first (or at all), but we can make a much better improvement to the program. Consider our very first example, `n = 24`. Its factors are `1, 2, 3, 4, 6, 8, 12, 24`. Can we detect anything else here that might help us? In most cases, recognizing a pattern is what leads us to make optimizations to our programs.

A factor is a number `x` such that `n % x == 0`. This means that there is another factor `y` such that `x * y = n`. If we find one factor, we can always calculate the other factor simply by dividing `n` by the first factor. For example, if `n = 24`, the second factor we find is `2`. We can calculate a second factor by `24 / 2 = 12`. So, now we know that we can find two factors simultaneously. But how do we implement this?

Let's consolidate what we have so far:

1. So far, we figured out that we only need to check `n / 2` values at most for factors.
2. But we can do better because we also figured out that for every factor we find, we can calculate an accompanying factor.
3. We need to figure out a way to iterate only `x / 2` times where `x` is the number of factors since we only need to find `x / 2` factors to calculate the rest.

## Explanation

To rephrase the problem given our new understanding of factors: we want to find all the unique `(x, y)` pairs such that `x * y = n`.

Looking at `x * y = n` doesn't tell us much. But what if we set `x = y`. Then we have a special relationship. If `x = y` then `x` and `y` are the square root of `n`. What happens if you increase `x`? If you increase `x`, `x * y` becomes bigger than `n`. What happens if you decrease `x`? `x * y` becomes smaller than `n`. What this means is that either `x` or `y` will **always** be `<= sqrt(n)`.

The two factors will only be equal if they are equal to `sqrt(n)`, otherwise one factor will always be bigger and the other one smaller. This means that we can get away with iterating only `sqrt(n)` times! We can basically look for all the "smaller" factors, i.e. factors that are less than `sqrt(n)` and calculate the bigger factor. Here's the code:

``` python
from functools import reduce

# less elegant
def factors(n):
    fList = []
    for i in range(1, int(n ** 0.5 + 1)):
    	if n % i == 0:
        	fList.append(int(i))
            fList.append(int(n / i))
    return set(fList)

# more elegant
def factors(n):    
    return set(reduce(list.__add__, 
                ([i, n//i] for i in range(1, int(n**0.5) + 1) if n % i == 0)))
```

Our `for` loop only runs `sqrt(n)` times now, thus the runtime is *O(sqrt(n))*. This runs blazingly fast even when `n` is very large (just look at a `y = x` vs `y = sqrt(x)` graph).