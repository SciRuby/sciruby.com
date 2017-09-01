---
layout: post
title: "GSoC 2017 : data visualization using daru-view"
date: 2017-09-01 22:46
comments: true
author: Shekhar Prasad Rajak
categories: [GSoC,GSoC 2017,Students,Data Visualization,daru,daru-view,
  Charts,Plotting,Data Analysis]
---


## Introduction

Daru is doing pretty good work as the data analysis & manipulation in IRuby notebook as well as backend part of web application. Ruby web application frameworks like Ruby on Rails, Sinatra, Nanoc are popular frameworks. So if Ruby developers get the gem like daru which can do data analysis
and visualization work in applications, then there is no need of shifting to another language or usage of other gem.

My project for <abbr title="Google Summer of Code 2017"> GSoC 2017</abbr> was to "make Daru more ready for integration with modern Web framework" in terms of visualization.

To improve in terms of viewing data,
[daru-view](https://github.com/Shekharrajak/daru-view), a plugin gem for
[daru](https://github.com/SciRuby/daru) is created. [daru-view](https://github.com/Shekharrajak/daru-view) is for easy and interactive plotting in web application & IRuby notebook. It can work in frameworks like Rails, Sinatra, Nanoc and hopefully in others too.

If you just want to see the features of daru-view, you can check these
examples :

* [IRuby Notebook Examples](http://nbviewer.jupyter.org/github/shekharrajak/daru-view/tree/master/spec/dummy_iruby/)

* [daru io and daru-view usage in Rails app](https://github.com/Shekharrajak/daru_examples_io_view_rails)

* [Readme of daru-view](https://github.com/Shekharrajak/daru-view)


## Design of daru-view

[daru-view](https://github.com/Shekharrajak/daru-view), currently using
[Nyaplot](https://github.com/SciRuby/nyaplot), [HighCharts](https://www.highcharts.com/), [GoogleCharts](https://developers.google.com/chart/interactive/docs/gallery) for plotting the charts. It is also
generating tables using [DataTables](https://datatables.net/) and [GoogleCharts](https://developers.google.com/chart/interactive/docs/gallery) with pagination, search and various features.


### Design Pattern in daru-view

daru-view mainly uses the [adapter design pattern](https://en.wikipedia.org/wiki/Adapter_pattern) and [composite design pattern](https://en.wikipedia.org/wiki/Composite_pattern).

* **Why Adapter design pattern:**

  * Adapter pattern’s motivation is that we can reuse existing gems if we can modify the interface.

  * daru-view joins functionalities of independent or incompatible interfaces of different gems.

* **Why Composite design pattern:**

  * To define common objects and use it for defining composite objects.


## Implementation

daru-view ensure that it's functions are usable in both IRuby notebook as well
as ruby web application frameworks.

The main thing we need to display something in web application or IRuby
notebook is `HTML` code of it. daru-view generates the `HTML` code od the
chart, table and the same can be used to display in web application & IRuby
notebook.

These are the libraries which is used in daru-view currently:

### Nyaplot

[Nyaplot](https://github.com/SciRuby/nyaplot) is good library for
visualization in IRuby notebook only. When we use Nyaplot as the adapter in
daru-view, it is usable in both IRuby notebook and web applications. Daru
DataFrame or Vector is used as the data source of the chart. It works
similar to the initial `daru` plotting system.


### HighCharts

To add the [HighCharts](https://www.highcharts.com/) features for plotting various chart types, daru-view uses the [lazy_high_charts](https://github.com/michelson/lazy_high_charts) gem with additional features.

In this adapter data source can be Array of data, daru DataFrame, Vector or HTML table code of the data.

There are various of options in HighCharts. One can see the options that can
be used in [HighCharts demo link](https://www.highcharts.com/demo), which can
be direclty used in daru-view Plot.

**HighCharts adaptor can work offline as well in daru-view. Developers can update the saved the JS files (in daru-view) using rake task automatically.**


### GoogleCharts




#### GoogleCharts as data table



### DataTables



## Future Work



## Conclusion



## Acknowledgements
