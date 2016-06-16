---
layout: post
title: Symbol to Proc
date: '2014-08-01 15:00:00'
tags:
- ruby
---

I was going through some Ruby coding exercises to improve my overall vocabulary and came across some pretty sweet syntactic sugar with Symbol#to_proc. Check this out:

```ruby
my_sentence = "The big Red DOG jumped over the fence"

def downcase_it(sentence)
  sentence.split(" ").map(&:downcase) # => Same as sentence.split(" ").map{ |n| n.downcase}
end

print downcase_it(my_sentence)

# => ["the", "big", "red", "dog", "jumped", "over", "the", "fence"]
```
This is a great solution for those relatively simple iterations you may need to do. I’m always in favor of typing less.