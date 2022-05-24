---
title: "First Post"
excerpt: ""
last_modified_at: 2022-05-06T17:05:09 #overrides date in name of post
tag:
	- Blog
	- Stego
	- Steganography
	- Polyglots
category:
author_profile: true
author: Èmilienne Zaiding
---

I ran into [stegosploit](https://stegosploit.info/) while working on a HTB box. It was definitely a rabbit hole but one that was quite fun. 

I couldn't find the source code anywhere, and in stego fashion had to extract it out of the pdf (which you can find [here](https://www.alchemistowl.org/pocorgtfo/pocorgtfo08.pdf)).

Once you read through the article you had to run:

```bash
unzip pocorgtfo08.pdf stegosploit_tool.png
```
Which gives us this image
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/Stegosploit_Screen_Capture.png" alt="">

If you follow the directions you end up getting a zip file when you click on the image you open in your browser.

