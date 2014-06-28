---
layout: post
title: "The progress of GSoC D3 Project"
date: 2014-06-28 11:00
comments: true
categories: [D3,GSOC,GSOC2014,Students,Visualization]
---
This week I submitted my mid-term evaluation of Google Summer of Code, and I finished the first-half of my GSoC term. Let me report the progress during the term.

![alt text](https://dl.dropboxusercontent.com/u/47978121/gsoc/nyaoplot_top.png)

## Progress on the front-end library
I am developing [Nyaplot](https://github.com/domitry/Nyaplot) as the front-end library for my work.

One topic on Nyaplot is API design. That is a difficult problem because each plotting library has very different way to create plot. For example, matplotlib and Bokeh are both written in Python but their demo code is written in very different styles. After discussing with my mentor, I decided to implement the function-based API. See the smallest code to generate Bar chart with Nyaplot:

```ruby
require 'nyaplot'
plot = Nyaplot::Plot.new
plot.add(:bar, ['nya1', 'nya2', 'nya3'],[10,20,30])
plot.show
```
If you want to change options (e.g. color or title), put the return value of Nyaplot::Plot.add into a variable and call its methods.

```ruby
bar = plot.add(:bar, ['nya1', 'nya2', 'nya3'],[10,20,30])
bar.title("the number of cats") # name the bar chart
```

Another topic on Nyaplot is the interaction with [IRuby](https://github.com/minad/iruby). IRuby is a web-based Ruby environment and it is important for fast prototyping.

The image on top of this blog post is a hyperbolic spiral which I generate with Nyaplot on IRuby. See [the notebook on nbviewer](http://nbviewer.ipython.org/github/domitry/Nyaplot/blob/master/examples/notebook/Introduction.ipynb), one of IRuby and IPython notebook hosting services.

## Progress on the back-end library
Nyaplot uses [Nyaplotjs](https://github.com/domitry/Nyaplotjs) as its back-end library. I spent a lot of time to implement interactivity among multiple panes to Nyaplotjs. See [an example](http://www.domitry.com/gsoc/).

![alt text](https://dl.dropboxusercontent.com/u/47978121/gsoc/top.png)

Its data source is a tsv file which contains 2044 lines. Multiple-panes interactivity is especially important when visualizing such a large dataset.

Have a look at radio bottoms beside Venn plot. The left three panels decide witch SET to put into each of 3 circles (VENN1, VENN2, VENN3). The right panel decides which data in each areas (overlapping area, non-overlapping area and all) to use in the other two panes.

The gray box on histogram desides the range of PNR value. If you select the range 0.3 to 0.7 with it, the right bar chart will reflect that and show how many of selected data are in that range.

Nyaplotjs provides this interactivity using event handler connected to the dataframe. That allows us to handle unidirectional update method (e.g. histogram update bar chart, but bar chart do not update histogram).

## Conclusion
I finished the first half term of Google Summer of Code 2014, but I still have a lot of things to do. I would like to continue to add interactivity to both front-end and back-end library.

I am developing those two libraries in separate repositories on GitHub. If you are interested in them, feel free to raise issues or send pull-request even during GSoC term.

+ [Nyaplot - GitHub](https://github.com/domitry/Nyaplot)
+ [Nyaplotjs - GitHub](https://github.com/domitry/Nyaplotjs)
