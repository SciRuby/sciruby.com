---
layout: post
title: "Updates: Minimization and Integration"
date: 2014-06-18 11:32
author: Rajat Kapoor
comments: true
categories: [GSOC2014,GSOC,Minimization,Integration,GSL,Students]
---
Minimization
------------
Unidimensional minimization gem now supports all the funcitons provided by the GSL minimization module. The supported methods include the pure ruby implementations of:
1. Newton Raphson Method
2. Golden Section
3. Brent
4. Quad Golden
out of which the Golden Section, Brent and Quad golden are also present in GSL based implementation. Everything is organized in such a way so as to execute the GSL or pure ruby methods based on whether gsl is available or not. I still have to beautify the code and add documentation.

Integration
-----------
The integration gem has been moved from hoe to bundler. For Gauss Kronrod quadrature I have hardcoded the values of nodes and weights as we already had hardcoded values in case of Gauss Quadrature. I have hardcoded the values for 15, 21, 31, 41, 61 point Quadrature. Apart from this basic methods like Simpsons 3/8 method, Milne's method, Boole's Quadrature and Open Trapeziod methods have been added. I will be reviewing a [pull request](https://github.com/clbustos/integration/pull/3) which aims to change the structure of the whole Integration gem and will continue the work accordind to the new structure. After that I plan to implement more adaptive methods and incorporate the non adaptive methods under a single, Newton-Cotes function. I am also brainstorming about how to proceed on the part involving symbolic integration using JScience and JRuby.