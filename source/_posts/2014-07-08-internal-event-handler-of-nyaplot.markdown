---
layout: post
title: "Internal event handler of Nyaplot"
date: 2014-07-08 16:11
comments: true
author: Naoki Nishida
categories: [D3,GSOC,GSOC2014,Students,Visualization,Nyaplot]
---

![Nyaplot Multi-pane Example](https://dl.dropboxusercontent.com/u/47978121/gsoc/multiple_panes.png)

## Introduction

Last week I implemented the multiple panes interactivity to the front-end of Nyaplot. You can see the demo on [this notebook](http://nbviewer.ipython.org/github/domitry/Nyaplot/blob/master/examples/notebook/Introduction.ipynb).

Skip over single pane demo, and have a look at the paragraph 'Multiple panes demo' at the bottom. In this example histogram and bar chart are created individually, and then thrown into one instance of Nyaplot::Frame. After running 'frame.show', both of the diagrams appear in one out-put box on the notebook. And when you click the histogram, bar chart refrects that and change its height.

In this article I will explain about the event handler of Nyaplot, which allows it the interactivity among panes.

## Trick

The trick is actually hidden in Nyaplot::DataFrame. In this example, both of two diagrams are generated from columns in one DataFrame. 
When Nyaplot receives column object (instance of Nyaplot::Series[0]), it internally find its source DataFrame and remember that.

Therefore Nyaplot can find that histogram and bar chart are sharing their data source, and allow them to interact each other.

![Nyaplot::DataFrame Example](https://dl.dropboxusercontent.com/u/47978121/gsoc/table.png)

## Implementation

Before explaining about event handler, I should talk how Nyaplot render its diagrams.
In the back-end JavaScript library, diagrams like histogram fetch column data from DataFrame and generate their SVG dom nodes dynamically.[1]

And the key is DataFrame written in JavaScript.[2] After mouse events coming from a filter(gray box on the plot area) are handled, histogram registers its filter function to DataFrame and tell all diagrams to update their SVG dom object.[3]
And bar chart updates its SVG from data filterd by the new function.[4]

That is all what Nyaplot is doing and is common in all diagrams supported by Nyaplot. So not only histogram and bar chart but other diagrams can do interactive behavior.(e.g. [hist, bar, and venn example](http://www.domitry.com/gsoc/multi_pane2.html))

## Conclusion

Dispite that its structure is very simple, the event handler system is working well. Actually interactivity in Nyaplot is based on DataFrame, both in Ruby front-end and JavaScript back-end. Therefore handling events in DataFrame is natural and efficient for Nyaplot.

## Notes
[0] [lib/nyaplot/data.rb#L115](https://github.com/domitry/nyaplot/blob/master/lib/nyaplot/data.rb#L115)  
[1] [src/view/diagrams/histogram.js#L20](https://github.com/domitry/Nyaplotjs/blob/master/src/view/diagrams/histogram.js#L20)  
[2] [src/view/utils/dataframe.js](https://github.com/domitry/Nyaplotjs/blob/master/src/utils/dataframe.js)  
[3] [src/view/diagrams/histogram.js#L96](https://github.com/domitry/Nyaplotjs/blob/master/src/view/diagrams/histogram.js#L96)  
[4] [src/view/diagrams/bar.js#L54](https://github.com/domitry/Nyaplotjs/blob/master/src/view/diagrams/bar.js#L54)  

## Additional Information
Nyaplot and its back-end library Nyaplotjs are being developed on GitHub. 

+ [Nyaplot on GitHub](https://github.com/domitry/nyaplot)
+ [Nyaplotjs on GitHub](https://github.com/domitry/Nyaplotjs)

If you are interesed, feel free to try it and raise issues or send pull-requests.
