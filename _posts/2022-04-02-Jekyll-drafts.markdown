---
layout: post
title:  "Working with drafts in Jekyll"
date:   2022-04-02 13:00:00 -0500
categories: 
---
After a six-month hiatus from this site, I could not remember much. I noticed I had a couple of posts in a `_drafts` folder within my site. Here's a refresher on working with drafts in Jekyll:

- **Location**
  - Drafts live in a `_drafts` subfolder.
- **Viewing** (no future dates)
  - To see drafts in a local build of your site that don't have a future date, add the `--drafts` flag when you run Jekyll, as follows (assuming you are using `bundle exec`): 
```bash
bundle exec jekyll serve --drafts
```
- Viewing **future-dated drafts**
  - If you date your drafts in the future, run Jekyll with the additional `--future` flag (thanks [Edgar!](https://uhded.com/jekyll-post-future-date)): 
```bash
bundle exec jekyll serve --drafts --future
```
- **Naming** a draft
  - The [Jekyll documentation](https://jekyllrb.com/docs/posts/) says—somewhat misleadingly—"Drafts are posts without a date in the filename." But my drafts *had* dates in the file names.
  - It turns out you *can* put a date in the filename, or in the file's front matter. Based on a [very helpful post from Redgreenrepeat](https://redgreenrepeat.com/2019/05/31/guide-to-working-with-drafts-in-jekyll/) (archived [at archive.org](https://web.archive.org/web/20190617154340/https://redgreenrepeat.com/2019/05/31/guide-to-working-with-drafts-in-jekyll/)), it seems that Jekyll uses these rules to save and display drafts:
    - With *no date* anywhere in the file, it will be stored and diplayed according to its *modified date*. (Per the Jekyll documentation, a non-dated draft "will be assigned the value modification time of the draft file for its date.")
    -  With a date in the *file name* but *not the front matter*, it will display at the date in the file name.
    -  With a date in the *front matter*, it will display at that date, regardless of whether there's a date in the file name.
- **Publishing** is simple: move the file to `_posts` and (if needed) rename it accordingly.
- **Troubleshooting**
  - If you aren't seeing a draft you know is there, you may need to:
    - Search for it
    - Manually enter a URL with the date that you think has been assigned to the draft
    - Use the `--futures` flag as noted above