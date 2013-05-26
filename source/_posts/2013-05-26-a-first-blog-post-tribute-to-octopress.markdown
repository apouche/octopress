---
layout: post
title: "A First Blog Post: Tribute to Octopress"
date: 2013-05-26 15:59
comments: true
categories: [html, jekyll, octopress]
---

As a Software Developer, it is now kind of trendy to maintain a blog where you can share a few gits or titbits of code with the community.

Fortunatly, there are many tools out there to ease the process of creating a blog. And when you don't know html5 that well and are not very good with CSS either, some of these tools will make your life easier.

The new trendy tool that I found during my research and that I decided to use for my blog is <a href="http://octopress.org/">octopress</a>.

<a href="http://octopress.org/"><img src="/images/posts/octopress.png" /></a>

<h3>Setup</h3>

For a basic setup (ie no database connection), the installation is very easy and if your a Mac user, it can be summarized into the following steps:

<ol>
	<li><a href="http://octopress.org/docs/setup/rvm/">Install</a> ruby 1.9.3</li>
	<li><a href="https://github.com/imathis/octopress">Fork</a> octopress</li>
	<li><a href="http://octopress.org/docs/configuring/">Configure</a> your blog</li>
	<li><a href="http://octopress.org/docs/blogging/">Start</a> blogging</li>
</ol>

Once your setup is done and you have your first post created, you simply have to generate the blog and preview it.

```
	rake generate
	rake preview
```
The preview command will start a webserver listenning on localhost:4000, it really is <b>THAT</b> simple.

<h3>Working with images</h3>



