---
layout: post
title:  "Nginx setup - configuration files (Part 1)"
date:   2021-11-18 22:32:01 -0500
categories: 
---

## Nginx configuration files - conventions

[Nginx](https://nginx.org/) is controlled by a set of plain-text configuration files. Instructions on the web about these files are confusing because the conventions about them have changed over time. According to a [2017 video from Nginx](https://www.youtube.com/watch?v=Ln1CPel3ALQ&t=466s), there are the main files and directories:

- `/etc/nginx/` - top-level directory for all configuration files
- `/etc/nginx/nginx.conf` - server-level configuration that "should not need much modification"
    - *What does "not much" modification mean? Surely this could be made clearer.*
    - *The [Nginx beginner's guide](https://nginx.org/en/docs/beginners_guide.html) misleadingly* tells you *to modify this file. Open source is awesome, but open-source documentation is not.*
- **`/etc/nginx/conf.d/`** - directory that holds site-specific `.conf` files, such as `mysite.conf`
    - files in this directory are pulled into the server configuration by this line inside `nginx.conf`:
        - `include /etc/nginx/conf.d/*.conf;`
- **`/etc/nginx/conf.d/default.conf`** - the operative site-specific configuration file when you first install Nginx

Unfortunately, you will see lots of underexplained references to two other configuration directories:

- `/etc/nginx/sites-available`, and
- `/etc/nginx/sites-enabled`

Apparently [in the distant past](https://stackoverflow.com/questions/11693135/multiple-websites-on-nginx-sites-available), these were your configuration directories. And [a Digital Ocean tutorial from 2018, validated in 2021](https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-18-04), still tells you to use them (without saying why). But here's what O'Reilly's *NGINX Cookbook* ([available as registration-ware from NGINX.com](https://www.nginx.com/resources/library/complete-nginx-cookbook/)) says:

> In some package repositories, this folder \[i.e., `/etc/nginx/conf.d`\]  is named
`sites-enabled`, and configuration files are linked from a folder named `sites-available`; **this convention is deprecated.**

So my task will be to understand, and explain, how Nginx configuration files work, following modern conventions. Stay tuned.
 
-------------------
## Motivation for this series on Nginx

Someday, I'm going to build a [Django](http://www.djangoproject.com/) site. There are lots of tutorials and instructions about how to develop one locally, but less information about deploying one (the [official Django docs on deployment](https://docs.djangoproject.com/en/3.2/howto/deployment/) are underwhelming).

From what I've found, it seems like deploying Django alongside [Nginx](https://nginx.org/) is a standard practice. And there are some detailed tutorials on deploying them together, like [this one from RealPython](https://realpython.com/django-nginx-gunicorn). But even the good tutorials tend to be lists of instructions, and some of the instructions about configuring Nginx are underexplained and even outdated. So I set out to get a deeper understanding of Nginx configuration and best practices.

## "Nginx" or "nginx" or "NGINX"?

The [Nginx open-source docs](https://nginx.org/en/docs/) use "nginx." The [commercial Nginx distro](https://www.nginx.com/) uses "NGINX." I use "Nginx" because it's the easiest to read and write.

