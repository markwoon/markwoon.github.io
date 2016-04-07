---
layout: post
title: Using Images on the Home Page of a Github Wiki
---

The syntax for adding an image to a Github wiki page is simple enough:

```markdown
![alt text](path/image.png)
```

Unless, of course, it's the home page (i.e. `Home.md`).  In that case, it's

```markdown
![alt text](wiki/path/image.png)
```

Obviously.

What the hell, Github?

I thought I was going crazy until I stumbled across [this gem](http://stackoverflow.com/a/14931420/1063501).
