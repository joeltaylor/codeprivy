---
layout: post
title: Diving into Gems
date: '2015-04-23 12:13:00'
tags:
- ruby
---

There have been a handful of times, while debugging, when I really need to know how a specific ruby gem is handling a request. In the past, I'd pop over to its Github repo and peruse the code from there, but that is generally  a bit disruptive to my normal work flow. Luckily, there's a better way!

`bundle open [gem]`

This pops me right into the source code of the gem that's being used in the project. From here, I can search around, tinker, and even add a `binding.pry` if I want.

The best part? After messing around with the gem, I can simply run `gem pristine [gem]` and the gem will be restored to its original state.

In the past, I usually steered away from thinking that a gem could be the root of a bug. Plus, opening Github tends to be tedious whenever I'm in the bug smashing zone. With `bundle open`, it's trivial to pop into a gem and more often than not it helps me understand the larger context. 