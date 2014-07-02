---
layout: post
title: "The progress of the GSoC D3/Visualization Project"
date: 2014-07-02 17:09
comments: true
author: Naoki Nishida
categories: [D3,GSOC,GSOC2014,Students,Visualization,Nyaplot]
---
This week is the half-way point of this year's Google Summer of Code. Let me report the progress during the term.

![alt text](https://dl.dropboxusercontent.com/u/47978121/gsoc/nyaoplot_top.png)

## Progress on the front-end library
I am developing [Nyaplot](https://github.com/domitry/Nyaplot) as the front-end library for my work.

One topic on Nyaplot is API design. That is a difficult problem since each plotting library has very different methods for creating plots. For example, matplotlib and Bokeh are both written in Python, but their demo code is written in very different styles. After discussing with my mentor, I decided to implement the function-based API.

As an exmaple of that API, here is a minimal working example for generating bar charts with Nyaplot:

```ruby
require 'nyaplot'
plot = Nyaplot::Plot.new
plot.add(:bar, ['nya1', 'nya2', 'nya3'],[10,20,30])
plot.show
```
If you want to change options (e.g., color or title), put the return value of Nyaplot::Plot.add into a variable and call its methods.

```ruby
bar = plot.add(:bar, ['nya1', 'nya2', 'nya3'],[10,20,30])
bar.title("the number of cats") # name the bar chart
```

## IRuby integration
Interaction with [IRuby](https://github.com/minad/iruby) is a priority for Nyaplot. IRuby is a web-based Ruby lab notebook-like environment, based on IPython, which is also useful for fast prototyping.

The image on top of this blog post is a hyperbolic spiral which I generated with Nyaplot in 
IRuby. [Have a look at the notebook](http://nbviewer.ipython.org/github/domitry/Nyaplot/blob/master/examples/notebook/Introduction.ipynb) on nbviewer, an IRuby and IPython notebook hosting services.

## Progress on the back-end library
Nyaplot uses [Nyaplotjs](https://github.com/domitry/Nyaplotjs) as its back-end library. I spent a lot of time to implement interactivity among multiple panes to Nyaplotjs. See [an example](http://www.domitry.com/gsoc/).

![alt text](https://dl.dropboxusercontent.com/u/47978121/gsoc/top.png)

The input data source is a TSV file of 2,044 lines. Multiple-pane interactivity is especially important when visualizing such a large dataset.

Have a look at the radio buttons beside the Venn diagram. The left three panels decide which set to put into each of 3 circles (VENN1, VENN2, VENN3). The right panel decides which data in each area (overlapping area, non-overlapping area, and all) to use in the other two panes.

The gray box on the histogram decides the range of PNR values. If you select the 
range 0.3 to 0.7 with it, the right bar chart will reflect that and show how many 
of the selected data are in that range.

Nyaplotjs provides this interactivity using an event handler connected to a dataframe object. That 
allows us to handle unidirectional update method (e.g., histogram updates bar chart, but bar chart does not update histogram).

## Conclusion
I finished the first half term of Google Summer of Code 2014, but I still have a lot of 
things to do. I would like to continue to add interactivity to both the front-end and back-end libraries.

I am developing those two libraries in separate repositories on GitHub. If you are interested, feel free to raise issues or send pull-requests, even during the GSoC term.

+ [Nyaplot - GitHub](https://github.com/domitry/Nyaplot)
+ [Nyaplotjs - GitHub](https://github.com/domitry/Nyaplotjs)

