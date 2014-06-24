---
layout: post
title: "Progress on Minimization methods"
date: 2014-06-19 01:00
author: Lahiru Lasandun
comments: true
categories: [GSOC2014,GSOC,Minimization,Students]
---
Current Progress on Minimization Gem
--------------------
In the first half of the summer, I plan to introduce some new numerical minimization methods to SciRuby's Minimization gem. As per my proposal, I began by implementing the Powell's multidimensional minimization method. Powell's method has a better convergence in most cases than the Nelder&ndash;Mead algorithm, and is also a multidimensional minimization method which doesn't use any derivative of the function.

I started by studying SciPy and Apache Commons library's Powell's optimizer. I decided to base my implementation on the method from the [Apache Commons Mathematics Library](http://commons.apache.org/proper/commons-math/). Powell's method requires a line minimum searching algorithm, for which I used Brent minimizer (already available in SciRuby).

Having finished with Powell's method, I am now working on the Fletcher--Reeves minimization method &mdash; a gradient method which uses the first derivative of the integrating function.
