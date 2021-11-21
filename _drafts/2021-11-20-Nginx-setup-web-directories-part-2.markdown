---
layout: post
title:  "Nginx setup - configuration files (Part 1)"
date:   2021-11-20 22:32:01 -0500
categories: 
---

## Website directories - conventions

Using Nginx to serve static files is essentially just a matter of mapping directories to requests. Before we map anything to anything, we need something to map—we need some kind of directory structure. And we need the right permissions for our directories and files. 

On my installation of Nginx, the default page (the one that says Nginx is working) is served from:
```bash
/usr/share/nginx/html/
```

But on a standard, non-shared *nix web server handling traffic for `somedomain.com`, the files are typically served out of a subdirectory of `/var/www/`, such as:
```bash
/var/www/somedomain.com/ # if you are serving multiple sites
/var/www/somedomain.com/html/ # if you are old school
/var/www/ # if you are serving just one site
/var/www/html/ # one site, old school
```
And when Nginx is used in combination with Django, the conventional location for serving Django-related static content (skipping over nuances for now) seems to be:
```bash
/var/www/somedomain.com/static/
```
(*Note:* If you're using shared hosting, you will only have access to your user-specific `home` directory, and your web server will serve files from some subdirectory of `home`, not a subdirectory of `var`.)

## Directories, ownership, and permissions - my s

For purposes of learning Nginx and getting it up and running:

- I'll use `/var/www/mydomain.com/` as my root directory.
- I'll create `/var/www/mydomain.com/static/` for later use with Django.
- For ownership and permissions, [following the canonical StackOverflow/ServerFault question on this issue](https://serverfault.com/questions/357108/what-permissions-should-my-website-files-folders-have-on-a-linux-webserver) I will:
    - Set 

