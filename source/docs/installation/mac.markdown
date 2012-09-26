---
layout: page
title: "Mac OSX Install Instructions"
date: 2012-04-11 15:05
comments: false
sharing: true
footer: true
---

GCC
---

Mac OSX typically comes with GCC 4.2 or earlier, which does not support the C++0x and C++11 extensions required by
NMatrix.

You can check this using
<pre><code>gcc -v</code></pre>

[Extremely detailed instructions](https://github.com/SciRuby/nmatrix/wiki/NMatrix-Installation) are available on the NMatrix
Wiki.

<h3>Homebrew</h3>

[Homebrew installation instructions are available here.](https://github.com/mxcl/homebrew/wiki/installation)

You can install GCC 4.6 using [this formula](https://github.com/Homebrew/homebrew-dupes/blob/master/gcc.rb), e.g.,

<pre><code>sudo brew install --enable-cxx --enable-fortran</code></pre>

However, we prefer the [script included in NMatrix](https://github.com/SciRuby/nmatrix/blob/master/scripts/mac-brew-gcc.sh),
which will give you GCC 4.7.1. That script and the command above both come from [a question on apple.stackexchange.com](http://apple.stackexchange.com/questions/38222/how-do-i-install-gcc-via-homebrew).

<h3>MacPorts</h3>

<pre><code>sudo port install gcc46
</code></pre>

These (un-tested) MacPorts instructions come from [a question on Stack Overflow](http://stackoverflow.com/questions/6210399/how-do-i-build-gcc-on-a-mac/6210459#6210459).

Green Shoes
-----------

You need to start by following the instructions for [installing Green Shoes on OSX](https://github.com/ashbb/green_shoes/wiki/Building-Green-Shoes-on-OSX).
