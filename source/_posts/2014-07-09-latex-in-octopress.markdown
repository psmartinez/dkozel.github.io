---
layout: post
title: "Latex in Octopress"
date: 2014-07-09 02:21:23 +0100
comments: true
categories: 
---

Many of the topics I work with involve math left, right, and center. Being able to use $$\LaTeX$$ is a big feature for me so here's the solution I've come up with so far. Credit certainly goes to  [Dr Zac](http://drz.ac/2013/01/03/blogging-with-math/) and [Felix](http://www.idryman.org/blog/2012/03/10/writing-math-equations-on-octopress/) for their work getting Latex playing nice. I believe that Zac's is the more correct solution given its use of the custom header file rather than modifying the default layout. However Felix has the fix for a bug in MathJax which otherwise turns the entire page white when viewing an equation's source.
<!--more-->

The first step is to change the Markdown processor to be kramdown (or pandoc if you prefer, see Dr Zac's page) as it supports Latex.

* In ```Gemfile``` replace ```rdiscount``` with ```kramdown``` and remove the version requirement.
* In ```_config.yml``` replace ```rdiscount``` with ```kramdown``` as the markdown parser
* Run ```bundle install``` to install Kramdown if necessary.

Now add the [MathJax](http://www.mathjax.org/) script to each page's header

``` plain source/_includes/custom/head.html
<!--- MathJax Configuration -->
<script type="text/javascript"
src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>
```

Finally fix the MathJax right click bug if necessary. I'm using the [readify](https://github.com/vladigleba/readify) theme which doesn't trigger the bug, however the default theme does.

``` sass sass/base/_theme.scss
body {
  > div {
    background: $sidebar-bg $noise-bg;
```

needs to be changed to

``` sass
body {
  > div#main {
    background: $sidebar-bg $noise-bg;
```

And the result, an attractive Fourier Transform equation

$$ F(\omega) = \int_{-\infty}^\infty f(t) e^{-i \omega t} dt. $$

