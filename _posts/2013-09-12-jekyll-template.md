---
layout: post
title: Jekyll Template
subtitle: Making a website with Jekyll and GitHub pages
photo: banner_700x420.jpg
photo-alt: example cover photo
icon: fa-lightbulb
icon-style: regular
category: [Web]
tags: [Jekyll]
---

# General 

This is a template built on [Chris Bobbe's](https://github.com/chrisbobbe/jekyll-theme-prologue) Jekyll conversion of the [HTML5 UP Prologue](https://html5up.net/prologue) theme. It has been modified to use GitHub Flavored Markdown (GFM) and rouge code highlighting.

# Front page

You can edit content on the frontpage by editing the files in __\_sections__. This can be either HTML or markdown.

# Jekyll variables

https://jekyllrb.com/docs/variables/

# Markdown

[Read More](https://www.markdownguide.org/extended-syntax/)

Definition List
: Definition 1
: Definition 2


# Code

Code is formatted with Rouge using GitHub style markdown:

```python
def main():
    pass
```

<span class="image right"><img src="{{ 'assets/images/banner_2.jpg' | relative_url }}" alt="" /></span>
![My helpful screenshot](/assets/images/banner_2.jpg)


## Anchors in markdown

You can create an anchor by using the hyperlink markdown with the section name in hyphenated lower case. For example the anchor for this section is:

```[my anchor text](#anchors-in-markdown)``` 

Which will appear as: [my anchor text](#anchors-in-markdown).


## Image

<span class="image right"><img src="{{ 'assets/images/pic03.jpg' | relative_url }}" alt="" /></span>

## Table

<div class="table-wrapper">
  <table>
    <thead>
      <tr>
        <th>Name</th>
        <th>Description</th>
        <th>Price</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Item 1</td>
        <td>Ante turpis integer aliquet porttitor.</td>
        <td>29.99</td>
      </tr>
      <tr>
        <td>Item 2</td>
        <td>Vis ac commodo adipiscing arcu aliquet.</td>
        <td>19.99</td>
      </tr>
      <tr>
        <td>Item 3</td>
        <td> Morbi faucibus arcu accumsan lorem.</td>
        <td>29.99</td>
      </tr>
      <tr>
        <td>Item 4</td>
        <td>Vitae integer tempus condimentum.</td>
        <td>19.99</td>
      </tr>
      <tr>
        <td>Item 5</td>
        <td>Ante turpis integer aliquet porttitor.</td>
        <td>29.99</td>
      </tr>
    </tbody>
    <tfoot>
      <tr>
        <td colspan="2"></td>
        <td>100.00</td>
      </tr>
    </tfoot>
  </table>
</div>
