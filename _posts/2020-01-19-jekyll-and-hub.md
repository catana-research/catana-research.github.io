---
layout: post
title: Jekyll and Hub
subtitle: Making a website with Jekyll and GitHub pages
photo: PlexusWire_3_2.jpg
photo-alt: example cover photo
category: [Web]
tags: [Jekyll, GitHub]
---

Jekyll is a scripting language that enables you to quickly build webpages using markdown. Combined with [GitHub pages](https://pages.github.com) you can develop and host your own custom website for free. 

In the following we will cover how to quickly setup and deploy your own website using Jekyll and GitHub.


### Initial setup

Head over to GitHub and create a new repository named _username.github.io_, where _username_ is your GitHub username (or organization name) on GitHub. If not already enabled, also make sure your account is setup to use [GitHub pages](https://pages.github.com).

Next, make a clone of the template repo:
```bash
git clone https://github.com/catana-research/catana-research.github.io.git
```
This will be a good base for your website which you can tweak later [(see below)](#tweaking-your-site).

Rename the old _origin_ to _source_ to enable pulling from the template repo and add your own repo:
```bash
git remote rename origin source
git remote add origin https://github.com/catana-research/catana-research.github.io.git
```


### Deploying the site

Before you can view your site you must first deploy it. This can either be done on your own machine *locally* or served to the world using _GitHub_.

#### Locally

With Jekyll you can easily deploying your website to your local machine, enabling you to quickly make and review changes before deploying to the web. Before deploying locally you must first install the packages for the the website. To do this, open a command prompt in the directory root and install the required website packages with:

```bash
bundle install
```
    
You will need to do this when you want to install new packages for your site.

You can then serve your site to your local machine with the command:

```bash
bundle exec jekyll serve
```

By default this will run a local server on port 4000: [http://127.0.0.1:4000](http://127.0.0.1:4000).


#### GitHub

To serve your website on GitHub you simply need to commit to master and push your changes to your GitHub repository, this will deploy to: [https://catana-research.github.io/](https://catana-research.github.io/).


### Tweaking your site

Customise  _config.yml to edit the theme

#### Code formating                     

The site uses rouge for code syntax highlighting, using the _monokai_ theme. You can generate a style for your favourate colour scheme by:

```bash
rougify style monokai > assets/css/syntax.css
```

If you want to [read more](https://bnhr.xyz/2017/03/25/add-syntax-highlighting-to-your-jekyll-site-with-rouge.html).


## Summary

By now you should know how to create and deploy your own website for free. 

#### Further reading

- [Jekyll cheatsheet](https://devhints.io/jekyll).
- [Jekyll variables](https://jekyllrb.com/docs/variables/).
- [Markdown cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).


