---
layout: page
title: "NMatrix"
date: 2012-04-11 13:07
comments: true
sharing: true
footer: true
---
We are pleased to announce the first <em>alpha</em> release (0.0.1) of NMatrix, a component of SciRuby.

NMatrix is an experimental linear algebra library for Ruby, written mostly in C. It can be used with or without SciRuby, but is part of the SciRuby project.

NMatrix was inspired by and based heavily upon [NArray](http://narray.rubyforge.org), by Masahiro Tanaka.

## Features

In addition to dense matrices, NMatrix supports two types of sparse storage (list-of-list and Yale).

We support the following data classes:

 * Byte (unsigned 8-bit integer)
 * Integer (8, 16, 32, and 64-bit)
 * Floating point (32 and 64-bit)
 * Complex (64 and 128-bit)
 * Rational (32, 64, and 128-bit)
 * Ruby objects

NMatrix makes use of and exposes the CBLAS API included in [ATLAS](http://math-atlas.sourceforge.net/). However, we have also written BLAS templates for NMatrix-supported data types that are not included in ATLAS or LAPACK (e.g., rationals).

## Installation

<pre><code>gem install nmatrix
</code></pre>

For complete installation instructions, please see the [SciRuby docs](/docs).

## Developers

The repositories in the SciRuby account on Github contain only stable code which is ready for release. Since we're in the
midst of a major refactoring, you're better off forking either [Chris Wailes' repository](http://github.com/chriswailes/nmatrix)
or [John Woods'](http://github.com/mohawkjohn/nmatrix).

Here's how you would clone the stable version:

<pre><code>git clone git://github.com/SciRuby/nmatrix.git
</code></pre>

And this is how you would get John Woods' version:

<pre><code>git clone git://github.com/mohawkjohn/nmatrix.git
</code></pre>

You may also want to play around with the NMatrix fork of rb-gsl:

<pre><code>git clone git://github.com/SciRuby/rb-gsl.git
</code></pre>

## Bugs

Visit the [NMatrix issue tracker](https://github.com/sciruby/nmatrix/issues).

Please also feel free to ask for help in our [Google Group](http://groups.google.com/group/sciruby-dev) or in our IRC channel, #sciruby, on Freenode.