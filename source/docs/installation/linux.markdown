---
layout: page
title: "*NIX Install Instructions"
date: 2012-04-11 15:05
comments: false
sharing: true
footer: true
---

System headers
--------------

These steps should work for Debian and Ubuntu flavors. If you're using something else, please <a href="mailto:john.woods@marcottelab.org">email John</a> with the steps you took.

<pre><code>apt-get update # update package sources
apt-get install libgtk2.0-dev librsvg2-dev libcairo2-dev
apt-get install libgtksourceview2.0-dev # optional, for code editor
</code></pre>

If you have problems, you might consult [the Green Shoes wiki](https://github.com/ashbb/green_shoes/wiki/Building-Green-Shoes-on-Ubuntu).

NMatrix
-------

ATLAS and LAPACK are required. Detailed instructions for installing these packages are available from [SciPy](http://www.scipy.org/Installing_SciPy/Linux#head-eecf834fad12bf7a625752528547588a93f8263c).

<pre><code>gem install nmatrix
</code></pre>

SciRuby
-------

<pre><code>gem install sciruby
gem install gtksourceview2 # optional, for code editor
gem install statsample-optimization # optional: rb-gsl, statistics2
</code></pre>
