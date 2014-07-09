---
layout: post
title: "The Nyaplot internal event handler"
date: 2014-07-09 17:01
comments: true
author: Naoki Nishida
categories: [D3,GSOC,GSOC2014,Students,Visualization,Nyaplot]
---

![Nyaplot multi-pane example](https://dl.dropboxusercontent.com/u/47978121/gsoc/multiple_panes.png)

## Introduction

Last week I implemented the multiple panes interactivity to the
front-end of Nyaplot. You can see the demo on [this notebook](http://nbviewer.ipython.org/github/domitry/Nyaplot/blob/master/examples/notebook/Introduction.ipynb).

Skip over single pane demo, and have a look at the paragraph 'Multiple
panes demo' at the bottom. In this example, the histogram and bar chart are
created individually, and then thrown into one instance of
Nyaplot::Frame. After running `frame.show`, both of the diagrams
appear in one out-put box on the notebook. When you click the
histogram, bar chart refrects that and change its height.

In this article I will explain about the event handler of Nyaplot,
which allows for interactivity among panes.

## The Trick

The trick is actually hidden in Nyaplot::DataFrame. In this example,
both of the two diagrams are generated from columns in one DataFrame.
When Nyaplot receives a column object (instance of `Nyaplot::Series`,
defined
[here](https://github.com/domitry/nyaplot/blob/master/lib/nyaplot/data.rb#L115)), it 
internally find its source data frame and remembers its location.

Consequently, Nyaplot can find that histogram and bar chart are
sharing their data source, and allows them to interact with each other.

![Nyaplot::DataFrame Example](https://dl.dropboxusercontent.com/u/47978121/gsoc/table.png)

## Implementation

Before explaining the event handler, I want to outline Nyaplot's
diagram rendering procedure. In the back-end JavaScript library,
diagrams, such as histograms, fetch column data from data frames and generate
their SVG DOM nodes dynamically ([source code](https://github.com/domitry/Nyaplotjs/blob/master/src/view/diagrams/histogram.js#L20)).

The key is [a data frame written in JavaScript](https://github.com/domitry/Nyaplotjs/blob/master/src/utils/dataframe.js). After mouse events
coming from a filter (such as the gray box on the plot area) are
handled, the histogram [registers its filter function to the data frame and instructs all diagrams to update their SVG DOM objects](https://github.com/domitry/Nyaplotjs/blob/master/src/view/diagrams/histogram.js#L96). The bar chart [updates its SVG from data filtered by the new function](https://github.com/domitry/Nyaplotjs/blob/master/src/view/diagrams/bar.js#L54).

As you can see, Nyaplot's inner workings are fairly simple; and these
workings are common to all diagrams supported by Nyaplot. So not only
the histogram and bar chart, but other diagrams as well, can behave
interactively (e.g., [histogram, bar, and Venn example](http://www.domitry.com/gsoc/multi_pane2.html)).

## Conclusion

Despite, or perhaps because of, the relative simplicity of the
structure, the event handler functions well. The actual interactivity
in Nyaplot is based on the data frame object, both in the
Ruby front-end and the JavaScript back-end. As a result, handling
events in DataFrame is both natural and efficient for Nyaplot.

## Additional Information
Nyaplot and its back-end library Nyaplotjs are being developed on GitHub. 

+ [Nyaplot on GitHub](https://github.com/domitry/nyaplot)
+ [Nyaplotjs on GitHub](https://github.com/domitry/Nyaplotjs)

If you are interesed, feel free to try it and raise issues or send pull-requests.
