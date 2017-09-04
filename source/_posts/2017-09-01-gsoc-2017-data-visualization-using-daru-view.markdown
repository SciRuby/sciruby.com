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

If user want to use the Nyaplot methods then it can be done in Nyaplot object,
which can be accessable using `daru_plot_obj.chart`.

i.e.

```ruby
daru_view_obj = Daru::View::Plot.new(
                  daru_dataframe, options={adapter: :nyaplot})
nyaplot_obj = daru_view_obj.chart

```

Now user can operate all the methods for Nyaplot object. Same thing is for
all other adapter in daru-view.


### HighCharts

To add the [HighCharts](https://www.highcharts.com/) features for plotting various chart types, daru-view uses the [lazy_high_charts](https://github.com/michelson/lazy_high_charts) gem with additional features.

In this adapter data source can be Array of data, daru DataFrame, Vector or HTML table code of the data.

There are various of options in HighCharts. One can see the options that can
be used in [HighCharts demo link](https://www.highcharts.com/demo), which can
be direclty used in daru-view Plot.

**HighCharts adaptor can work offline as well in daru-view. Developers can update the saved the JS files (in daru-view) using rake task automatically.**

If you is familiar with `lazy_high_chart` gem and want to use it for
config the chart then user can access the `lazy_high_chart` object using
`Daru::View::Plot#chart` and can do necessary operations.


### GoogleCharts

To add the [GoogleCharts](https://developers.google.com/chart/interactive/docs/gallery) features for plotting various chart types, daru-view uses the [google_visualr](https://github.com/winston/google_visualr/) gem with additional features(in this module more new features are updated).

We want GoogleChart adapter to be very strong since Google chart tools always gets updated and it has amazing plotting features. Similar to the HighCharts module, here also we can use all the options described in Google Charts website.

User can access the `google_visualr` object using `Daru::View::Plot#chart`, if
they want to operate `google_visualr` methods.


#### GoogleCharts as data table

One of the good thing about googe chart is, it can be used for generating table
for web applcation and IRuby Notebook with pagination and other features.

**`Daru::View::Plot` can take data Array, daru DataFrame, Daru Vector,
Daru::View::Table as data source.**

**`Daru::View::Table` can take data Array, daru DataFrame, Daru Vector as data
 source.**


### DataTables

[DataTables](https://datatables.net/) has interaction controls to any HTML table. It can handle large set of data and have many cool features.

To use it, daru-view uses [https://github.com/Shekharrajak/data_tables](https://github.com/Shekharrajak/data_tables) gem. [Note: the gem name will be changed in near future]

It basically uses the HTML table code and add features that user want. So internally HTML table code of daru DataFrame and daru Vector is passed
as data source parameter.


## Future Work

daru-view will be more powerful and simple in near future. Developers can add
more libraris in daru-view easily, if required.

## Conclusion

The aim of the daru-view is to plot charts in IRuby notebook and ruby web
application easily, so that developers need not have to use any other gem or
language for visualization.

It can work smoothly in Rails/Sinatra/Nanoc web frameworks and I hope it can work in other ruby frameworks as well, because daru-view is generating the html code and javascript code for the chart, which is basic need of the webpage.

**Why not use the plotting libraries directly?**

If you are using daru gem for analysing the data and want to visualise it, then it will be good if you have data-visualisation within daru and can plot it directly using DataFrame/Vector objects of daru.

daru-view will be helpful in plotting charts and tables directly from the Daru::DataFrame and Daru::Vector . daru-view using nyaplot, highcharts , google charts right now to plot the chart. So user can set the plotting library and get the chart accordingly.

Most of the plotting libraries doesn't provide the features of plotting charts in iruby notebook. They are defined only for web applications (mostly for Rails). But daru-view can plot charts in any ruby web application as well as iruby notebook.

## Acknowledgements

I would like to thank to my mentors [Sameer Deshmukh](https://github.com/v0dro)
,[Lokesh Sharma](https://github.com/lokeshh) and [Victor Shepelev](https://github.com/zverok) for their response and support.

I thank my fellow GSoC participants [Athitya Kumar](https://github.com/athityakumar) and [Prasun Anand](https://github.com/prasunanand) for their support and discussions on various topics.

