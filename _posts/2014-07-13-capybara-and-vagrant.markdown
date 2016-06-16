---
layout: post
title: Capybara and Vagrant
date: '2014-07-13 18:00:00'
tags:
- rails
- vagrant
- capybara
---

Here’s a quick-tip for Vagrant users:

One of my favorite tools that Capybara has to offer is `save_and_open_page` for those times when you’re left wondering where exactly your fictional user ended up. Unfortunately, when using a VM without a GUI, there’s no browser to open the page in.

Luckily, there’s a really simple workaround. In your test helper add `Capybara.save_and_open_page_path = "/vagrant/debug_pages"` and the page will now be saved as HTML in your /vagrant directory. Of course, you can name the holding directory anything you want. In your tests you’ll need to use `save_page` to avoid the browser error and find the page saved in your specified directory.

Keep on keeping vagrant up.