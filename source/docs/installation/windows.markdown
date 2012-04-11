---
layout: page
title: "Windows Install Instructions"
date: 2012-04-11 15:05
comments: false
sharing: true
footer: true
---

None of the developers use Windows, so we cannot guarantee that these instructions will work. Please [join our listserv](http://groups.google.com/group/sciruby-dev) if you're a Windows user, and we'll be happy to help troubleshoot.

Green Shoes
-----------

You need to start by following the instructions for [installing Green Shoes on Windows 7](https://github.com/ashbb/green_shoes/wiki/Building-Green-Shoes-on-Windows-7).

NMatrix
-------

ATLAS and LAPACK are required. Detailed instructions for installing these packages are available from [SciPy](http://www.scipy.org/Installing_SciPy/Windows#head-cd37d819e333227e327079e4c2a2298daf625624).

<pre><code>gem install nmatrix
</code></pre>

SciRuby
-------

<pre><code>gem install sciruby
gem install gtksourceview2 # optional, for code editor
gem install statsample-optimization # optional: rb-gsl, statistics2
</code></pre>
