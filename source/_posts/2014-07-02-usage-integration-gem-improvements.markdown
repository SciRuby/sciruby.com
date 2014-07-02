---
layout: post
title: "Integration | Usage"
date: 2014-07-02 17:43
author: Rajat Kapoor
comments: true
categories: [GSOC2014,GSOC,Integration,Students]
---
Numerical Integration
---------------------
Integration is the process of finding the summation of the value of a function at small intervals, when the width of the intervals is infinitesimally small. 
As there is no such thing as "infinitesimally small interval", there are several other methods which help us to approximate the values of integrals. 

Numerical integration is one such technique which helps us to estimate the value of the definite integral of a function over an interval.
Numerical integration is based on the principle of evaluating the values of a function at specific points in the interval and then taking the product of those values with a corresponding weight, which is a standard constant specific to the method being used.

Integration Gem
---------------
The Integration gem is now equipped with a host of additional algorithms for approximating the integral of a function ranging from the simple ones like Simpson's method to the more complex ones like Gauss&ndash;Kronrod and Adaptive Quadrature method. 

Example
-------
Let us say you need to find out the integral of a function `5*x**2 -7*Math.cos(x)` over the interval `(0,1)`.
We can solve this using the following snippet using the Integration gem:

```ruby
Integration.integrate(0,1,{:method=>:simpson}) {|x| 5*x**2 -7*Math.cos(x)}
# => -4.223630227110514
```

We see that the actual value of this integral is `-4.2236302269886088799000849584`, which is pretty close to the value estimated by the integration gem.
Instead of `{:method=>:simpson}` in the above code, you can use any one of these implemented methods: 

```ruby
rectangle,:trapezoid,:simpson, :adaptive_quadrature , :gauss, :romberg, :monte_carlo, :gauss_kronrod, :simpson3by8, :boole, :open_trapezoid, :milne
```

Each of these algorithms gives similar results &mdash; except if the function has rapid changes in the interval of integration, as is the nature of numerical integration.

