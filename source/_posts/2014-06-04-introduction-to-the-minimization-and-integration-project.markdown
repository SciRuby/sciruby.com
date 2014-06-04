---
layout: post
title: "Introduction to the Minimization and Integration project"
date: 2014-06-04 14:23
author: Rajat Kapoor
comments: true
categories: [GSOC 2014,GSOC, Minimization,Integration,GSL]
---

Introduction to the Minimization and Integration Project 
--------------------------------------------------------
Hi. My name is Rajat Kapoor and I have been selected to work with SciRuby for Google Summer of Code 2014.
[Minimization](https://github.com/SciRuby/minimization) and [Integration](https://github.com/SciRuby/integration) are two of the many available gems in the SciRuby Suite. My project this year aims to improve these gems to replicate the functionality provided by GNU's GSL. I would be trying to implement all the minimization and integration algorithms present in GSL in pure Ruby so that these functions are easily accesible to all Ruby users, while the users which have GSL already installed will have an advantage in terms of speedy computations.

What Minimization and Integration actually means
------------------------------------------------
Minimization refers to the process of finding out the minimum of a mathematical function whose values might depend on multiple variables. Unidimensional minimization restricts these problems to functions of one single variable.
Integration is very widely used concept in calculus which basically boils down to finding the summation of the value of a function at small intervals, when the width of the intervals in infinitesimally small. I can bet that you knowingly or unknowingly use both these things on a daily basis.

The plan
--------
The project can be broken into two major parts: Minimization and Integration, as these are two seperate gems.
The Minimization gem can be broken in two parts: Unidimensional(or Univariate) and Multidimensional (Multivariate). With respect to coding, these two can again be broken down into subparts: pure Ruby implementations and GSL support.
The Integration gem will be having the pure Ruby implementations as well as GSL support. Along with this, support for symbolic integration will be added for the JRuby users(using the JScience library). The difference between symbolic integration and numerical integration will be made clearer in future posts.

Progress
--------
The pure Ruby implementations of the unidimensional Minimization part are almost finished. I am also working on the GSL support for the same along with it. I plan to finish up any unidimensional minimization work by the end of this week and start the work with multidimensional minimization methods. Keep watching this place for more updates regarding this projects as well as other GSoC projects
