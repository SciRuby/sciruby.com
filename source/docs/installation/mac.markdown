---
layout: page
title: "Mac OSX Install Instructions"
date: 2012-04-11 15:05
comments: false
sharing: true
footer: true
---

Green Shoes
-----------

You need to start by following the instructions for [installing Green Shoes on OSX](https://github.com/ashbb/green_shoes/wiki/Building-Green-Shoes-on-OSX).

NMatrix
-------

ATLAS and LAPACK are required. These should be included in Apple's Developer Tools.

<pre><code>gem install nmatrix
</code></pre>

SciRuby
-------

<pre><code>gem install sciruby
gem install gtksourceview2 # optional, for code editor
gem install statsample-optimization # optional: rb-gsl, statistics2
</code></pre>
