---
layout: post
title:  "Modifying a Jekyll theme to use a favicon (GHP Part 3)"
date:   2021-11-13 01:02:01 -0500
categories: 
---
(For previous posts in this series, [see the list at the bottom of the page](#previous-posts).)

When you first run `bundle exec jekyll serve` after setting up your site, Jekyll will complain that your site lacks a **favicon.** Let's fix that.

Putting a `favicon.ico` file in your site folder will silence the complaint—but it won't actually cause the favicon to be displayed anywhere. For that, you'll have to muck around with the Jekyll theme files. 

## Theme basics

According to the [Jekyll documentation](https://jekyllrb.com/docs/themes/), changing your site's theme is pretty simple. Rather than editing the theme's files, you selectively override them. The default theme, `minima`, is a Ruby gem, and it is installed with other gems rather than in your site folder. Jekyll knows to use the `minima` gem's files when it builds your site. 

To override the gem's files, you first need to know what, and where, they are. To find their location, run this command (if you're not using `minima`, substitute your theme's name): 
```
bundle info --path minima
```

Then navigate to that directory, figure out what you want to override, replicate the relevant directory structure in your site folder, and copy the files you want to modify into your site folder in the appropriate directory. Basically, if Jekyll finds a file in your site folder with the same name and relative location as a file in the theme gem's files, it will use the file in your site folder instead of the gem's file. 

## Enabling a favicon

Per [a very good post by Paul Cochrane about using favicons with Jekyll](https://ptc-it.de/add-favicon-to-mm-jekyll-site/), the best place for one is in an `assets/images` folder in your site folder. So I put my `favicon.ico` there.

Generally, a link to a favicon goes in the `<head>` element of an HTML document. If you're using the `minima` theme, the code for the `<head>` element of your site's pages lives in `_includes/head.html` in the theme's gem files. Per [a StackOverflow post on the subject](https://stackoverflow.com/questions/35037482/favicon-with-github-pages), it seems like the best code to put inside the `<head>` tag in the `head.html` document looks like this:
- for a .png file:
    ```
    <link rel="shortcut icon" type="image/png" 
        href="{{ "/assets/images/favicon.png"  | absolute_url }}">
    ```
- for an .ico file:
    ```
    <link rel="shortcut icon" type="image/x-icon" 
        href="{{ "/assets/images/favicon.ico"  | absolute_url }}">
    ```
So enabling the favicon, if you don't run into stupid problems caused by your code editor (see below), is as simple as:
1. putting a `favicon.ico` (or `.png`, etc.) file in `assets/images` in your site folder;
1. copying `_includes/head.html` over into your site folder; and 
1. editing the file to add the appropriate `<link>` tag, as shown above. 

--------
*Note*: I'm using **VS Code**, and trying to override the `minima` theme's `head.html` document **broke my CSS.** 

The reason was subtle and stupid: **VS Code screwed up the `head.html` code upon save.** It was inserting a space after a quotation mark in the path to my CSS file and my favicon. (A Jekyll error message gave a clue, as did inspecting the rendered HTML using my browser's developer tools.) 

As a temporary solution, I edited my `settings.json` file in VS Code to include this language-specific section ([thanks Flavio Copes for the template](https://flaviocopes.com/vscode-language-specific-settings/)):
```json
"[html]": {
        "editor.insertSpaces": false,
        "editor.formatOnSave": false,
        "editor.tabSize": 4
    },
```
I don't feel great about this solution, which I found in [an underwhelming StackOverflow post](https://stackoverflow.com/questions/40817613/vscode-inserting-spaces-via-format-on-save). I'd like to have *some* format-on-save capability, and it seems like disabling `insertSpaces` alone should fix the problem. But it doesn't. So this will have to do for now.

-----------
## Previous posts
- Part 1: [A GitHub Page about setting up GitHub Pages](/2021/11/06/github-page-about-github-pages.html)
- Part 1.1: [Reinstalling Jekyll and Bundler](/2021/11/11/reinstalling-jekyll-and-bundler.html)
- Part 2: [GitHub Pages concepts](/2021/11/07/github-pages-part2-concepts.html) 