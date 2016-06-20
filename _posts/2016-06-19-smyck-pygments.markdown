---
layout: post
title: Smyck Pygments Theme
date: '2016-06-19 20:00:00'
tags:
- Ruby
---

I've been unable to find a Pygments syntax theme that I like and figured I'd
find a way to convert my current Vim theme over [(Smyck \<3)](https://github.com/hukl/Smyck-Color-Scheme).

Fortunately, before I went down the rabbit hole of manually creating the theme, a search
led me to [vim2pygments](https://github.com/honza/vim2pygments). You feed it a Vim
color scheme and it spits out a Pygments file equivalent. After that, you can use
the `pygmentize` tool form Pygments to create the CSS file.

If you're into Smyck, I've put it up on Github [here.](https://github.com/joeltaylor/smyck_pygment)

##### Ruby example:

```ruby
class Smyck
  module PygmentsTheme
    GREAT_FOR = [:jekyll, :things]

    def self.explain
      puts "Smyck theme converted over to pygemnts"
    end

    # @return [Array] of apparent emotions
    def emotions
      %w(happy bananas)
    end
  end
end
```
