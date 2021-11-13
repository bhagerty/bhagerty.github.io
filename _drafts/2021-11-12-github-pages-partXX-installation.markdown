---
layout: post
title:  "Jekyll installation for GitHub Pages  (GHP Part 3)"
date:   2021-11-11 13:02:01 -0500
categories: 
---
By this point, we've installed [Ruby](https://www.ruby-lang.org/en/) and [Jekyll](https://jekyllrb.com/) under the [Windows Subsystem for Linux (WSL)](https://docs.microsoft.com/en-us/windows/wsl/about) and Ubuntu 20.04. (See [Part 1](/2021/11/06/github-page-about-github-pages.html) about GitHub Pages.) And we understand what GitHub Pages is. (See [Part 2](/2021/11/07/github-pages-part2-concepts.html).) 

It's time to build a local Jekyll site that we'll be pushing to GitHub. Naturally, we'll follow instructions at Jekyll. But ***which* instructions**? There are two sets, a [Quickstart](https://jekyllrb.com/docs/) and a [Step-by-Step Tutorial](https://jekyllrb.com/docs/step-by-step/01-setup/), and they aren't even close to the same! 

Examining how the instructions differ was a good way for me to learn a little more about Ruby app, so I'm going to do my best to explain what I learned.

## The Quickstart Instructions

Per the a [Quickstart](https://jekyllrb.com/docs/), creating a Jekyll site is basically a two-step process. 

- **Step 1.** From a command line prompt with the directory you want to be the **parent** of your Jekyll site, run:
    ```bash
    jekyll new myblog
    ```
    This creates a new subdirectory, `myblog`, that has the ingredients of your Jekyll site. 

- **Step 2.** Change (`cd`) into that directory and, with one command, build the site, spin up a local server to deliver it, and spin up a process to watch for changes to source files (and rebuild the site on the fly if they happen):
    ```bash
    bundle exec jekyll serve
    ```
    Or (if you want the the site to be both rebuilt *and* refreshed in your browser when it changes):
    ```bash
    bundle exec jekyll serve --livereload
    ```
    Note: To just build the site without spinning up a server or watching for changes , you would run:
    ```bash
    bundle exec jekyll build
    ```
If you're wondering, "Why run just plain `jekyll` the first time but `bundle exec jekyll` the second time?" we think alike. I'll get there.

## The Step-by-Step Tutorial

The [Step-by-Step Tutorial](https://jekyllrb.com/docs/step-by-step/01-setup/) has completely different instructions! 






-----------
PS: Ideally, I would suggest some changes to the Jekyll docs, which are open source. And maybe I will!