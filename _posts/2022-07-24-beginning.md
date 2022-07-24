---
layout: post
title: First Post
date: 2022-07-21 12:00:00 -400
cagegories: [homelab, documentaion]
tags: [web,vscode,git,testing]
---

# Welcome

Hello and welcome to my homelab docs site - this page was created to use as a template of examples for various kinds of markdown and formatting I can use on the site
<br><br><br>
# H1 - Heading Test
## H2 - Heading Test
### H3 - Heading Test
#### H4 - Heading Test
##### H5 - Heading Test

<br><br><br>

## What is Lorem Ipsum? 
Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book. It has survived not only five centuries, but also the leap into electronic typesetting, remaining essentially unchanged. It was popularised in the 1960s with the release of Letraset sheets containing Lorem Ipsum passages, and more recently with desktop publishing software like Aldus PageMaker including versions of Lorem Ipsum.

## Why do we use it?
It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout. The point of using Lorem Ipsum is that it has a more-or-less normal distribution of letters, as opposed to using 'Content here, content here', making it look like readable English. Many desktop publishing packages and web page editors now use Lorem Ipsum as their default model text, and a search for 'lorem ipsum' will uncover many web sites still in their infancy. Various versions have evolved over the years, sometimes by accident, sometimes on purpose (injected humour and the like).

## Lists
* one
    * <b>BOLD TEXT</b>
    * <i>ITALIC TEXT</i>
* two
* three
* four

### More Lists w. colored text
1) <span style="color:red">red</span>
2) <span style="color:blue">blue</span>
3) <span style="color:green ">green</span>

## Code bock examples
### Javascript code block
```javascript
console.log('hello world!");
```
### YML code block
```yml
name: 'push-remote'

on:
  push:
    branches:
      - master
    paths-ignore:
      - .gitignore
      - README.md
      - LICENSE
```
### Bash/Terminal/Console code block
```bash
sudo apt update && sudo apt upgrade -y
```

# Embedded tips
This is an example of a tip
> Add Markdown syntax content to file `_tabs/about.md`{: .filepath } and it will show up on this page.
{: .prompt-tip }