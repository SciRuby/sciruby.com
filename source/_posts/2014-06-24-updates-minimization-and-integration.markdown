---
layout: post
title: "Updates: Minimization and Integration"
date: 2014-06-24 14:32
author: Rajat Kapoor
comments: true
categories: [GSOC2014,GSOC,Minimization,Integration,GSL,Students]
---

Minimization
------------

The Minimization gem now supports the following unidimensional function minimizations
provided by GSL. The supported methods include the pure Ruby implementations of:

1. Newton&ndash;Raphson
2. Golden Section
3. Brent
4. Quad Golden

Of these, the Golden Section, Brent, and Quad Golden are also
available via Minimization's GSL interface (and are thus
faster). Everything is organized in such a way that the faster C code
(i.e., GSL) will be executed when GSL is available, but that otherwise
the Ruby implementation will be used. I still have to beautify the
code and add documentation.

Integration
-----------

The Integration gem has been transitioned from Hoe to Bundler. For
Gauss&ndash;Kronrod Quadrature, I have hard-coded the values of nodes
and weights (for 15, 21, 31, 41, and 61 points) --- which were already
hardcoded in the case of the Gauss quadrature.

Additionally, I added basic methods like Simpson's Three-Eighths
Method, Milne's Method, Boole's Quadrature and Open Trapezoid.

This week, I will be reviewing a [pull request](https://github.com/clbustos/integration/pull/3) which aims to change the structure of the whole Integration gem.

After that I plan to implement more adaptive methods and incorporate
the non-adaptive methods under a single Newton&ndash;Cotes function.

Lastly, I am brainstorming designs for symbolic integration using
JScience and JRuby.
