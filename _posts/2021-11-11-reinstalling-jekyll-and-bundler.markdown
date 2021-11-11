---
layout: post
title:  "Reinstalling Jekyll and Bundler (GHP Part 1.1)"
date:   2021-11-11 11:40:01 -0500
categories: 
---
In my [first post](/2021/11/06/github-page-about-github-pages.html) about setting up [Ruby](https://www.ruby-lang.org/en/) and [Jekyll](https://jekyllrb.com/) on the [Windows Subsystem for Linux (WSL)](https://docs.microsoft.com/en-us/windows/wsl/about) and Ubuntu 20.04, **I made a mistake**: I used `sudo` to install a bunch of Ruby-related "gems," including `bundler`, a crucial tool for running Ruby apps, and `jekyll`. 

This caused problems later, so I decided to unwind things. Two terrific blog posts by [Moncef Belyamani](https://www.moncefbelyamani.com/) helped me understand both why and how to do so:
- one explaining [why you should never use sudo to install ruby gems](https://www.moncefbelyamani.com/why-you-should-never-use-sudo-to-install-ruby-gems/), and
- his [definitive guide to installing ruby gems on a mac](https://www.moncefbelyamani.com/the-definitive-guide-to-installing-ruby-gems-on-a-mac/). (I'm using Ubuntu under WSL, so some adaptations were needed.)

In the ruby-installation guide, Moncef recommends using a Ruby installation manager, such as `rbenv` or `chruby` (see [the Ruby docs](https://www.ruby-lang.org/en/documentation/installation/#installers)). 

But for me, that ship had sailed. I had already installed Ruby systemwide using the Ubuntu package manager (`apt-get install` etc.). I didn't want to unwind that. So I chose Moncef's third, non-recommended option: 

- **Enable user-based (non-`sudo`) installation of gems despite having a system-wide install of Ruby.** 

I have no desire to be a Ruby developer, so this was the right approach for me, since it meant that I didn't need to entirely reinstall Ruby. This worked for me:

- **Step 1**: Get rid of `bundler` and `jekyll`.
    ```bash
    sudo gem uninstall jekyll bundler
    ```
    - I got a bunch of prompts and warnings to deal with, but they all made sense, and I uninstalled everything that I was able to uninstall.
- **Step 2**: In home directory (`cd ~`) create a `.gemrc` file, and update the `.bashrc` file, to enable user-based installation of gems. 
    - The `.gemrc` file is only one line (this specifies that gems will be installed in user space, not systemwide):
        ```
        gem: --user-install
        ```
    - The `.bashrc` file needs two lines, which update your path and create an environment variable (*note*: you may have to use a different version number for Ruby in the PATH statement; check the directory in your system):
        ```
        export GEM_HOME="$HOME/.gem"
        export PATH="$HOME/.gem/ruby/2.7.0/bin:$PATH"
        ```
    - Force `bash` to reread the `.bashrc` file:
        ```
        source ~/.bashrc
        ```
- **Step 3**: As regular user, *not* `sudo`, reinstall `bundler` and `jekyll` and update gems (order of commands doesn't matter here):
    ```
    gem update
    gem install bundler
    gem install jekyll
    ```
    - This makes these gems available to the *user*, but not yet to a particular *project,* such as my Jekyll site (i.e., the directory in which my GitHub Pages site is found).

- **Step 4**: Update *project* so that it uses the newly installed user-space gems (`jekyll` and `bundler`). Within project directory:
    ```
    bundle install
    ```
That was it! Since the Jekyll project already included a `Gemfile` and a `Gemfile.lock` that documented all the gems needed for the project, `bundle install` was able to read those files and find what it needed. To preview:
```
bundle exec jekyll serve
```
