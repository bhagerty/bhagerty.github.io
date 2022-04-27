---
layout: post
title:  "A GitHub Page about setting up GitHub Pages (GHP Part 1)"
date:   2021-11-06 18:40:01 -0500
categories: jekyll, ruby, wsl
---
There's lots of information online about how to use [GitHub](https://docs.github.com/en/pages) Pages and [Jekyll](https://jekyllrb.com/) to serve a website, but inevitably, the information is incomplete. Here's some of what I learned while getting my GitHub Pages Jekyll site going.

**Issue 1: Windows.** I'm on Windows. Jekyll is a [Ruby](https://www.ruby-lang.org/en/) app that comes from the world of Mac developers, so Windows "is not an officially supported platform." 

That seems crazy, especially now that we have [Windows Subsystem for Linux (WSL or WSL2)](https://docs.microsoft.com/en-us/windows/wsl/about), which lets me run Ubuntu on my Windows box. So I set about using WSL to install Ruby, so I could install Jekyll, so I could use GitHub Pages. (You don't *need* to use Jekyll with GitHub Pages, but it seems like the thing to do.) 

**Issue 2: Ruby.** Installing Ruby on WSL2 running Ubuntu should be straightforward, right? Just a little `sudo apt install` ~~(or `sudo apt-get install`)~~<sup id="a1">[1](#f1)</sup> this and that and you're there, right? 

Not so fast, cowboy. Yes, you can `sudo apt install ruby`, but the [Jekyll docs](https://jekyllrb.com/docs/installation/windows/) tell you to get Ruby from a specialized repo, [BrightBox](https://www.brightbox.com/docs/ruby/ubuntu/), not from the generic Ubuntu repo.<sup id="a2">[2](#f2)</sup> So my installation of Ruby and other prerequisites (after a false start and `sudo apt remove ruby`) started like this:
```bash
$ sudo apt install gcc
$ sudo apt install make
$ sudo apt install make-doc
$ sudo apt-add-repository ppa:brightbox/ruby-ng
$ sudo apt install ruby2.7 ruby2.7-dev build-essential dh-autoreconf
```
- *Note*: Check the available Ruby version at [BrightBox](https://www.brightbox.com/docs/ruby/ubuntu/) and update accordingly. The Jekyll docs are outdated, referring to 2.5 even though 2.7 is available.

At this point, the Jekyll docs tell you to run `gem update`. Again, not so fast! I did that and got a bunch of errors. I [posted on StackOverflow](https://stackoverflow.com/questions/69866170/how-do-i-provide-configuration-options-for-a-ruby-makefile-wsl-ubuntu-20-04) about the errors, but the short version is to avoid them, before you `gem update`, you should probably do:
```bash
$ sudo apt install libssl-dev
$ sudo apt install zlib1g-dev
$ sudo apt install libffi-dev
$ sudo apt install libreadline-dev 
$ sudo apt install libedit-dev
$ gem install io-wait -v 0.1.0 
```
Once this is done, go ahead and run `gem udpate` ~~or `sudo gem update`~~. 
- ~~[The Jekyll docs suggest](https://jekyllrb.com/docs/troubleshooting/#no-sudo) that using `sudo` is a bad idea because you end up installing packages systemwide. But it's what I did.~~
- I got an error about `io-wait` that I ignored because I don't think it's fixable, and I hope it won't matter. 

**Update**: Using `sudo` to install or update gems truly is a terrible idea, as [Moncef Belyamani explains](https://www.moncefbelyamani.com/why-you-should-never-use-sudo-to-install-ruby-gems/). So I had to unwind some of what I did. I made a [new post, part 1.1 of this series,]() about how I unwound it. 


Next, you can install `bundler` and `jekyll` in a one-line statement:
```bash
$ gem install jekyll bundler
```
That's it for now. [Part 2 is next](/2021/11/07/github-pages-part2-concepts.html). 

---
<sup id="f1">1</sup>You'll see both `apt install` and `apt-get install` in tutorials, with no explanation of the difference. Basically, [**use `apt install`**](https://web.archive.org/web/20220415192936/https://phoenixnap.com/kb/apt-vs-apt-get). The `apt-get` command is outdated.[↩](#a1)

<sup id="f2">2</sup>Actually, the Jekyll installation instructions are inconsistent. The [Ubuntu-specific instructions](https://jekyllrb.com/docs/installation/ubuntu/) tell you to just use `apt` and grab Ruby from the default repo. But the [WSL instructions](https://jekyllrb.com/docs/installation/windows/#installation-via-bash-on-windows-10) tell you to use Brightbox, even though WSL (by default) **is also Ubuntu**. So there's no principled reason for different instructions. [↩](#a2)

---

Updated 2022-04-27