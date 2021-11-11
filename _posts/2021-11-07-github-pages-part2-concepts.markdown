---
layout: post
title:  "GitHub Pages concepts (GHP Part 2)"
date:   2021-11-07 13:02:01 -0500
categories: 
---
In my [first post](/2021/11/06/github-page-about-github-pages.html) about GitHub Pages, I jumped right into setting up [Ruby](https://www.ruby-lang.org/en/) and [Jekyll](https://jekyllrb.com/) on Windows, using the [Windows Subsystem for Linux (WSL)](https://docs.microsoft.com/en-us/windows/wsl/about) and Ubuntu 20.04. 

Here, I'm backing up to the conceptual level, to answer two questions: 

1. **What *is* GitHub Pages?** and
1. **What is a GitHub Pages *site?*** 

**Answer 1.** GitHub Pages is set of server-side tools that can *build and deploy a website* using Jekyll (which is built on Ruby). Let's break that down:

- Why do I say "server-side tools"? Don't we install Jekyll locally?
    - Yes, we install Jekyll locally to get a site's scaffolding in place. But once we have done that, ***we do not have to*** **build** ***a site locally.***
    - Put another way, after we write our posts and pages within our local Jekyll directories and push them up to GitHub, GitHub Pages deploys our pages to our site by ***building the site on the server.*** We *do not* push a built site to GitHub. (If you look at your `.gitignore` folder in your local Jekyll site, you'll see that `_sites`, the built site, is excluded.) We just push the site's building blocks. 
    - Practically, this means that you don't have to build your site locally. You can, of course, if you want to preview it. But all you really need to do locally (assuming you want to work locally) is write your individual pieces of Jekyll content. GitHub Pages will then build and deploy them, once you push them to GitHub, using magic (actually, [GitHub Actions](https://docs.github.com/en/actions) that [invoke Jekyll](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/about-jekyll-build-errors-for-github-pages-sites), I think).
- Why do I say "can" build and deploy a site? 
    - Because you don't necessarily have to organize your pages according to Jekyll's structure. You can hand-code your HTML or Markdown if you want.
    - GitHub Pages will still run Jekyll to build and deploy your site, but it will do it from your hand-coded pages.

**Answer 2.** There are *two different types* of GitHub Pages sites:

1. First, there are **user-based** sites like this one, with a URL of the form:
    - **`username.github.io`**.
2. Second, there are **repo-based** sites, with a URL of the form: 
    - **`username.github.io/reponame`** (or maybe **`username.github.io/reponame/foldername`**)

This distinction is important, but I haven't seen it clearly explained. Lots of tutorials seem to tell you to initiate your GitHub Pages site by creating a folder in a repo. But that is only for a **repo-based** site. 

If you want to make a **user-based** site, you don't create a folder in an existing repo. You create a **new repo** with a **magic name**, as follows:

- The repo *itself* must be named **`username.github.io`**. See [step 3 in GitHub's directions.](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll)
- You do not need to create any subfolders. If you want to serve your site from **`username.github.io`**, your code should live right in the root of this new repo. 
- You do not need to give your code branch any special name; "main" ([not "master"](https://github.com/github/renaming)) is fine. I happened to call my branch "gh-pages" because a tutorial suggested doing so. I now realize that this was pointless. 