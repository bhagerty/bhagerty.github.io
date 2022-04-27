---
layout: post
title:  "Footnotes in Markdown"
date:   2022-04-27 18:02:01 -0500
categories: markdown
---
Markdown is weirdly limited, and you can't easily create footnotes. You can, however, fake them.<a id="a1"></a><sup>[1](#f1)</sup> [This StackOverflow post](https://stackoverflow.com/questions/25579868/how-to-add-footnotes-to-github-flavoured-markdown) is a good starting point.

Conceptually, the process is simple: using HTML tags, create a note number and a footnote that link to one another internally. The key is the HTML `id` attribute, which can be added to any tag. (Some folks use `name` rather than `id`, but I found that `name` didn't work within the VS Code Markdown previewer, so I use `id`.)

Concrete examples of adding `id` to **arbitrary formatting tags** (`a1` is the `id` of the number; `f1` is the `id` of the note — these names are arbitrary):

- Note number in text:
```Markdown
<sup id="a1">[1](#f1)</sup>
```
- Footnote, with return link:
```Markdown
<sup id="f1">1</sup>Note text goes here. [↩](#a1)
```

Concrete examples of using **anchor tags**, thus separating linking and formatting information:

- Note number in text:
```Markdown
<a id="a1"></a><sup>[1](#f1)</sup>
```
- Footnote at bottom of page, with return link:
```Markdown
<a id="f1"></a><sup>1</sup>Note text goes here. [↩](#a1)
```

Conceptually, although anchor tags are more verbose, I like them because they are explicit (and [explicit is better than implicit](https://miguelgfierro.com/blog/2018/python-pro-tips-understanding-explicit-is-better-than-implicit/#:~:text=Explicit%20is%20better%20than%20implicit%20Being%20explicit%20means,not%20to%20hide%20the%20behavior%20of%20a%20function.)).

---
<a id="f1"></a><sup>1</sup>You might be asking yourself, "How is it possible that we are still hand-coding HTML in 2022? Did a time machine take me back to the 1990s?" Both good questions.[↩](#a1)
