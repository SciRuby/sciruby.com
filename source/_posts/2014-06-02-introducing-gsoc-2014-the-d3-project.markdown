---
layout: post
title: "Introducing the GSoC 2014 D3 Project"
date: 2014-06-02 14:00
comments: true
author: Naoki Nishida
editor: John Woods
categories: [D3,GSOC,GSOC2014,Students,Visualization]
---

Hello. I am Naoki, one of four Google Summer of Code (GSoC) 2014
students in SciRuby. Let me introduce my project. The goal of the GSoC
2014 D3 Project is to create a new plotting library for
SciRuby. [D3.js](http://d3js.org/) is the most suitable JavaScript
library to achieve this goal.

There are several non-Ruby plotting software libraries in the wild,
like ggplot, matplotlib, and ggplot2. Actually, SciRuby already has
its own plotting libraries named
[Plotrb](https://github.com/SciRuby/plotrb) and
[Rubyvis](https://github.com/SciRuby/rubyvis). The main feature of my
project compared with those software packages is _interactivity_.
Interactivity has various meanings here: interactivity when generating
plots, interactivity when viewing them, and server&ndash;client
interactivity. My project includes all of those.

My project can be divided into two components, one JavaScript and the
other Ruby. JavaScript serves as a back-end, and Ruby as a
front-end. I'm currently working on the former part. Have a look at a
few examples I've assembled:

* [Multiple panes](http://bl.ocks.org/domitry/b8785f02f36deef567ce)
* [Bar chart](http://bl.ocks.org/domitry/2f53781449025f772676)
* [Line](http://bl.ocks.org/domitry/e9a914b78f3a576ed3bb)
* [Scatter](http://bl.ocks.org/domitry/308e27d8d12c1374e61f)
* [Another theme](http://bl.ocks.org/domitry/f215d5ff3bd3f5fec2ad)

This project involves a number of challenges, but I believe it to be
achievable during this Summer of Code. Thank you for reading!
