---
layout: post
title: 'Test post and first post :D'
published: true
---

Note: This is my first post. I won't be adding syntax highlighting or many images yet.

I wanted to start this blog because my friend was asking me how to solve coding problems. I figured that documenting my approach to a particular problem would be helpful to them (and myself since writing encourages me to develop a deeper understanding of what I'm doing).

The first problem I want to talk about is a simple one: What's the most efficient way to find all the factors of a number? For this problem, I'll use Python 3.0.

First, let's define a factor. A factor of a number `n` is `i` if `i` divides `n` without leaving a remainder. That is, if `n % i == 0` we say that `i` is a factor of `n`.


##Pseudocode

Let's