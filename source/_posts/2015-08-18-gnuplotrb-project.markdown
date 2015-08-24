---
layout: post
title: "GnuplotRB and GSoC 2015"
date: 2015-08-18 23:26
comments: true
author: Ivan Evgrafov
categories: [GSOC,GSOC2015,Students,Visualization,GnuplotRB]
---

## Introduction

This summer I've been participating in Google Summer of Code
 with [GnuplotRB project](https://github.com/dilcom/gnuplotrb) (plotting tool for Ruby users based on [Gnuplot](http://www.gnuplot.info/))
 for [SciRuby](http://sciruby.com/). GSoC is almost over and I'm releasing v0.3.1 of GnuplotRB as a [gem](https://rubygems.org/gems/gnuplotrb/).
 In this blog post I want to introduce the gem and highlight some of its capabilities.

## Features

There are several existing plotting tools for Ruby such as Nyaplot, Plotrb, Rubyvis
 and Gnuplot gem. However they are not designed for large datasets and have fewer
 plotting styles and options than Gnuplot. Gnuplot gem was developed long ago and nowadays consists
 mostly of hacks and does not support modern Gnuplot features such as multiplot.

Therefore my goal was to develop new gem for Gnuplot which would allow full use of its
 features in Ruby. I was inspired to build an easy-to-use interface for the most commonly
 used features of Gnuplot and allow users to customize their plots with
 Gnuplot options as easily as possible in Rubyesque way.

<!--more-->

### 2D and 3D plots

The main feature of every plotting tool is its ability to plot graphs. GnuplotRB allows you
 to plot both mathematical formula  and (huge) sets of data. GnuplotRB supports plotting
 2D graphs (`GnuplotRB::Plot` class)  in Cartesian/parametric/polar coordinates and 3D
 graphs (`GnuplotRB::Splot` class) &mdash; in Cartesian/cylindrical/spherical coordinates.

There are vast of plotting styles supported by GnuplotRB:

- `points`
- `lines`
- `histograms`
- `boxerrorbars`
- `circles`
- `boxes`
- `filledcurves`
- `vectors`
- `heatmap`
- etc (full list in [gnuplot doc](http://www.gnuplot.info/docs_5.0/gnuplot.pdf) p. 47)

Plot examples:

<img src="{{ site.url }}/images/gnuplotrb-gsoc2015/plots.jpg" alt="Plot example" style="width: 100%;"/>

For code examples please see
 [the repository README](https://github.com/dilcom/gnuplotrb/blob/master/README.rdoc),
 [notebooks](https://github.com/dilcom/gnuplotrb/blob/master/notebooks/README.rdoc)
 and [the examples folder](https://github.com/dilcom/gnuplotrb/tree/master/examples).

### Multiplot

`GnuplotRB::Multiplot` allows users to place several plots on a single layout and output
 them at once (e.g., to a PNG file).
 [Multiplot notebook](http://nbviewer.ipython.org/github/dilcom/gnuplotrb/blob/master/notebooks/multiplot_layout.ipynb).

Here is a multiplot example
 (taken from [Sameer's notebook](http://nbviewer.ipython.org/github/SciRuby/sciruby-notebooks/blob/master/Data%20Analysis/Analyzing%20baby%20names/Use%20Case%20-%20Daru%20for%20analyzing%20baby%20names%20data.ipynb)):

<img src="{{ site.url }}/images/gnuplotrb-gsoc2015/multiplot.jpg" alt="Multiplot example" style="width: 80%; align: middle;"/>

### Animated plots

GnuplotRB may output any plot to gif file but `GnuplotRB::Animation` allows
 to make this gif animated. It takes several `Plot` or `Splot` objects just as
 multiplot does and outputs them one-by-one as frames of gif animation.
 [Animation notebook](http://nbviewer.ipython.org/github/dilcom/gnuplotrb/blob/master/notebooks/animated_plots.ipynb).

<img src="{{ site.url }}/images/gnuplotrb-gsoc2015/trajectory.gif" alt="Trajectory example" style="width: 80%; align: middle;"/>

### Fit

Although the main GnuplotRB's purpose is to provide you with swift, robust and
 easy-to-use plotting tool, it also offers a `Fit` module that contains several
 methods for fitting given data with a function. See examples in [Fit notebook](http://nbviewer.ipython.org/github/dilcom/gnuplotrb/blob/master/notebooks/fitting_data.ipynb).

### Integration with other SciRuby tools

#### Embedding plots into iRuby notebooks

GnuplotRB plots may be embedded into iRuby notebooks as JPEG/PNG/SVG
 images, as ASCII art or GIF animations (`Animation` class). This functionality
 explained in a special [iRuby notebook](http://nbviewer.ipython.org/github/dilcom/gnuplotrb/blob/master/notebooks/basic_usage.ipynb).

#### Using data from Daru containers

To link GnuplotRB with other SciRuby tools I implemented plot
 creation from data given in Daru containers (`Daru::Dataframe` and `Daru::Vector`).
 One can use `daru` gem in order to work with statistical SciRuby gems
 and plotting with GnuplotRB. Notebooks with examples: [1](http://nbviewer.ipython.org/github/dilcom/gnuplotrb/blob/master/notebooks/plotting_from_daru.ipynb), [2](http://nbviewer.ipython.org/github/dilcom/gnuplotrb/blob/master/notebooks/time_series_from_daru.ipynb).

### Possible datasources for plots

You can pass to Plot (or Splot or Dataset) constructor data in the following forms:

- String containing mathematical formula (e.g., `'sin(x)'`)
- String containing name of file with data (e.g., `'points.data'`)
- Some Ruby object responding to `#to_gnuplot_points`
  - `Array`
  - `Daru::Dataframe`
  - `Daru::Vector`

See examples in [notebooks](https://github.com/dilcom/gnuplotrb/blob/master/notebooks/README.rdoc#possible-datasources).

## Links

- [Project repository](https://github.com/dilcom/gnuplotrb/)
- [Gem page on Rubygems](https://rubygems.org/gems/gnuplotrb/)
- [Gem documentation on Rubydoc](http://www.rubydoc.info/gems/gnuplotrb/0.3.1)
- [Blog of the project](http://dilcom.github.io/gnuplotrb/)
- [Examples](https://github.com/dilcom/gnuplotrb/tree/master/examples)
- [iRuby notebooks](https://github.com/dilcom/gnuplotrb/blob/master/notebooks/README.rdoc)
