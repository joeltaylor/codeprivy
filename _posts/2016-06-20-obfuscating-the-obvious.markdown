---
layout: post
title: Obfuscating the Obvious
date: '2016-06-20'
tags:
- Ruby
- Refactoring
---

If you write Ruby, then you know the self-documenting benefit that idiomatic
code provides. By writing methods with single responsibilities, that have obvious outcomes,
you can rest assured that you're _probably_ writing decent code.

Let's look at a quick example using the [Hamming](http://exercism.io/exercises/ruby/hamming/readme) exercise from [Exercism.io](http://exercism.io).

The task is to create a method that will calculate the "Hamming distance" between two strands
of DNA.
In short, you find this value by comparing two DNA strands and counting how many of the nucleotides are different from their equivalent in the other string.

```ruby
GAGCCTACTAACGGGAT
CATCGTAATGACGGCCT
^ ^ ^  ^ ^    ^^
```

The Hamming distance between these two DNA strands is 7 since there are 7 occurrences
where the two DNA strands differ.

Essentially, we need a method that accepts two strings and will count the differences.

Ruby's String object comes with `chars` which will return an array of characters in the
string. That's a good starting point.

```ruby
class Hamming
  def self.compute(a, b)
    puts a.chars.inspect
    puts b.chars.inspect
  end
end

Hamming.compute("GAGCCTACTAACGGGAT","CATCGTAATGACGGCCT")
# ["G", "A", "G", "C", "C", "T", "A", "C", "T", "A", "A", "C", "G", "G", "G", "A", "T"]
# ["C", "A", "T", "C", "G", "T", "A", "A", "T", "G", "A", "C", "G", "G", "C", "C", "T"]
```

Right now, we're not doing anything special. Just by looking at this method, you can
infer that the outcome will be two arrays containing strings being printed in the console.

Now that we have two arrays, we need a way to compare each index. One way to do this
is to first `zip` the arrays together.

```ruby
class Hamming
  def self.compute(a, b)
    puts a.chars.zip(b.chars).inspect
  end
end

Hamming.compute("GAGCCTACTAACGGGAT","CATCGTAATGACGGCCT")
# [["G", "C"], ["A", "A"], ["G", "T"], ["C", "C"], ["C", "G"], ["T", "T"], ["A", "A"], ["C", "A"], ["T", "T"], ["A", "G"], ["A", "A"], ["C", "C"], ["G", "G"], ["G", "G"], ["G", "C"], ["A", "C"], ["T", "T"]]
```

Now that we have the strands paired together, we can easily count the differences.

```ruby
class Hamming
  def self.compute(a, b)
    a.chars.zip(b.chars).count { |s| s[0] != s[1] }
  end
end

Hamming.compute("GAGCCTACTAACGGGAT","CATCGTAATGACGGCCT")
# 7
```

We're almost finished with the solution. Next we need make sure we only accept
DNA strands that have equal length. If the lengths are different, we cannot compute the Hamming distance.

```ruby
class Hamming
  def self.compute(a, b)
    raise ArgumentError, "DNA lengths don't match" unless a.length == b.length

    a.chars.zip(b.chars).count { |s| s[0] != s[1] }
  end
end
```

You could argue that this code is self-documenting. We're using idiomatic Ruby methods
and the solution can practically be read as a sentence - "Take the characters from string
`a` then pair them together with the characters of string `b` and count the differences."

But is it _actually_ self-documenting? Or is it just self-documenting for someone
who has background knowledge of the exercise? Even if someone knows about this exercise,
chances are they might scratch their head at what those short variables represent before realizing
they're just obscure short-hand.

A lot of times, it's _really_ tempting to use short-hand variable names in your methods.
After all, you know what `a` and `b` are...but those variable names tell future developers
nothing about the method. What about that `s` variable we use within the `count` block?
Block variables are usually short, but they also usually match a common idiom.

For instance, using `k` and `v` make sense when iterating over a hash.

```ruby
some_hash = {a: 1, b:2}
some_hash.each {|k, v| puts "Key #{k} has the value #{v}."}
# Key a has the value 1.
# Key b has the value 2.
```

In our example, `s` has absolutely no meaning.

While using short variable names is a common and seemingly trivial choice, it ultimately
obfuscates your code. Let's fix that.

```ruby
class Hamming
  def self.compute(strand_a, strand_b)
    raise ArgumentError, "DNA lengths don't match" unless strand_a.length == strand_b.length

    strand_a.chars.zip(strand_b.chars).count { |strand| strand[0] != strand[1] }
  end
end
```

At this point, the method reads more clearly than before. As a rule of thumb, whenever
there are moments that you _assume_ a developer can _infer_ meaning or intent, you
are obfuscating something that should be made obvious.

While our present solution is relatively self-documenting, there's still a bit guesswork involved.
The only way for another developer to know what `strand_a` and `strand_b` are would
be to know that `chars` is a String method or to know about the exercise. This is where comment documentation
comes into play. There are many flavors of code documentation - I'll use the one specified
in the [Ruby style guide](https://github.com/styleguide/ruby/documentation)

```ruby
class Hamming
  # Public: Computes the Hamming distance between two DNA strands.
  #
  # strand_a - The first String to be compared to.
  # strand_b - The second String to be compared with.
  #
  # Returns the difference count Fixnum.
  def self.compute(strand_a, strand_b)
    raise ArgumentError, "DNA lengths don't match" unless strand_a.length == strand_b.length

    strand_a.chars.zip(strand_b.chars).count { |strand| strand[0] != strand[1] }
  end
end
```

I get that code should be self-documenting and understand the notion behind quality code
not needing documentation, but why pass on an opportunity to make code explicitly
obvious?

More and more I've begun to realize that taking shortcuts to save time now generally leads,
to soaking up my time or another developer's when it comes time to maintain
the debt introduced by my obscurities.
