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


## Design of daru-view

[daru-view](https://github.com/Shekharrajak/daru-view), currently using
[Nyaplot](https://github.com/SciRuby/nyaplot), [HighCharts](https://www.highcharts.com/), [GoogleCharts](https://developers.google.com/chart/interactive/docs/gallery) for plotting the charts. It is also
generating tables using [DataTables](https://datatables.net/) and [GoogleCharts](https://developers.google.com/chart/interactive/docs/gallery) with pagination, search and various features.


### Design Pattern in daru-view

daru-view mainly uses the [adapter design pattern](https://en.wikipedia.org/wiki/Adapter_pattern) and [composite design pattern](https://en.wikipedia.org/wiki/Composite_pattern).

#### Why Adapter design pattern:

1. Adapter pattern’s motivation is that we can reuse existing gems if we can modify the interface.

2. daru-view joins functionalities of independent or incompatible interfaces of different gems.

#### Why Composite design pattern:

1. To define common objects and use it for defining composite objects.


## Implementation




### Nyaplot




### HighCharts




### GoogleCharts




#### GoogleCharts as data table



### DataTables



## Future Work



## Conclusion



## Acknowledgements
