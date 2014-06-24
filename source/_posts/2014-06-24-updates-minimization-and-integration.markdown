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
1. Newton&ndash;Raphson method
2. golden section
3. Brent
4. quad golden

Of these, the golden section, Brent, and quad golden are also
available via Minimization's GSL interface (and are thus
faster). Everything is organized in such a way that the faster C code
(i.e., GSL) will be executed when GSL is available, but that otherwise
the Ruby implementation will be used. I still have to beautify the
code and add documentation.

Integration
-----------

The Integration gem has been transitioned from Hoe to Bundler. For
Gauss&ndash;Kronrod quadrature, I have hard-coded the values of nodes
and weights (for 15, 21, 31, 41, and 61 points) --- which were already
hardcoded in the case of the Gauss quadrature.

Additionally, I added basic methods like Simpson's three-eighths
method, Milne's method, Boole's quadrature and open trapeziod.

This week, I will be reviewing a [pull
request](https://github.com/clbustos/integration/pull/3) which aims to
change the structure of the whole Integration gem.

After that I plan to implement more adaptive methods and incorporate
the non adaptive methods under a single, Newton&ndash;Cotes function.

Lastly, I am brainstorming on designs for symbolic integration using
JScience and JRuby.
