---
title: "Stegosploit"
excerpt: ""
date: 2022-05-24 #overrides date in name of post
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

## Getting the source
Once you read through the article you had to run:

```bash
unzip pocorgtfo08.pdf stegosploit_tool.png
```
Which gives us this image
<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/Stegosploit_Screen_Capture.png" alt="">

It asks you to change the file to .html and open it in a browser. Once you do that you click on the image that opened in your browser and it downloads a zip file titled `stegosploit-tools.7z`. 

Once you unzip it you are left with a simple folder structure.

Of interest for this post is `/stego/iterative_encoding.html`
If you open this in your browser you are given a very rudimentary application for encoding payloads into images. 

To input a file you have to provide the full path in the Input file box and then 
make a few selections and input your exploit code.

<img src="{{ site.url }}{{ site.baseurl }}/assets/images/posts/Encode.png" alt="">


## How it works

Stegosploit is a combination of the two words stenography and exploit. 
The basic idea behind this to exploit browser vulnerabilities to run arbitrary javascript code. The interesting thing about stegosploit is that it is built to use a seemingly innocuous  method, an image file. Most of the information here comes out of the pocorgtfo pdf listed above. 

But it isn't just an image file. It is a polyglot file, including both an image an html (and javascript). If opened in an image viewer the encoded file shows a working image. If opened in a vulnerable browser it still shows the image but also exploits the code. 

Specifically, Stegosploit, was a POC built around Use- After-Free exploit (CVE-2014-0282). This is quite old but the idea behind it is solid. 

Initial attempts used HTML5 Canvas decoding, which is/was not detected as it is/was only performing canvas pixel manipulation. There were a few goals that make this interesting:<sup> pg. 29</sup>

1. No data to be transmitted over the network except JPG or PNG files
2. No visible aberrations to the image
3. No exploit code in strings within image
4. No user interaction
5. Only one image used

Managing to pack these five things together results in a stealthy exploit. It requires no interaction other than seeing the photo, the photo doesn't look wrong. 

To avoid aberrations the image is encoded on less significant bits (you can do least significant bits, but you can move a few layers up and still not get visual aberrations).

They also encoded in a grid with every nth pixel being used for encoding the bit stream.<sup> pg. 32</sup>


This exploit also uses the fact that browsers are more complex then image viewers (ie they have to recognize more file types). A browser uses two things (according to this article)
1. The file extension
2. HTTP Content-Type response header

Browsers have attempted to implement Content Sniffing which is where the vulnerability that polyglot exploits exists. 

In a generic sense, this exploit looked at the internals of different web browsers and looked for weaknesses in how they identified files and then exploited that weakness. 


There is definitely space for this type of research, I am not sure of current capabilities regarding image encoding, but this interests me and I expect to continue looking. 

