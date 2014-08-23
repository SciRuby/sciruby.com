---
layout: post
title: "Nyaplot: interactive plots generateor with Ruby"
date: 2014-08-23 10:07
comments: true
author: Naoki Nishida
categories: [D3,GSOC,GSOC2014,Students,Visualization,Nyaplot]
---

<iframe width="853" height="480" src="//www.youtube.com/embed/ZxjqsIluM88" frameborder="0" allowfullscreen></iframe>

## Introduction
Three months of GSoC 2014 term was over this week and finally I released my product [Nyaplot as a gem](http://rubygems.org/gems/nyaplot).
This blog post was written to introduce Nyaplot version 0.1.1.

## Feature
There are various plotting libraries in the world as ggplot2 from R community and [Plotrb](https://github.com/zuhao/plotrb) and [Rubyvis](https://github.com/clbustos/rubyvis) from Ruby.
The features of Nyaplot compared with those libraries are **interactivity**, **simplicity**, and **extendibility**.

Interactivity is the main theme of my d3 project in GSoC 2014.
You can soon find it by touching plots with your mouse or trackpad.
This feature is also available on browsers bundled with Android or iOS thanks to technologies like SVG and WebGL.

However the word 'interactivity' is not limited to situations like above.
You can feel that by using Nyaplot on [IRuby notebook](https://github.com/minad/iruby).
Various modules prepared by Nyaplot help you to create plots interacitively on the notebook.
You can also publish the result quickly by uploading the notebook to your Dropbox storage, gist, or somewhere else.
Here is [an example](http://nbviewer.ipython.org/gist/mgiraldo/a68b53175ce5892531bc) which Mauricio Giraldo created and published on gist.

Simplicity is also an important element.
Many plotting libraries has MATLAB-like function-based API but Nyaplot does not.
Nyaplot is designed in more Ruby-like object-oriented style, and its plots consist of various Objects created from different Classes.
But Nyaplot has simple shortcut methods and users need not to feel the difference seriously.

![Extension libraries](https://dl.dropboxusercontent.com/u/47978121/gsoc/extensions_for_blog_post.png)

Nyaplot can be widely extended with some JavaScript code.
It bundles 3 extention libraries in its gem package: Nyaplot3D for 3D plot, Mapnya for map visualization, and Bionya for circular plots inspired by circos.
Their structure is very simple and you can write the extension easily if you have a little experience in JavaScript and Ruby.
For example the Ruby part of Bionya has only about 100 lines even if its style is far from of standard plots.

## Quick start
You can install Nyaplot by simply running `gem install nyaplot`.
After that, find example code from `path_to_gems/nyaplot-0.1.1/examples/rb` and run some of them using `ruby` command.
These scripts will generate some plots with nyaplot and export them as html files to current directory.

In addition I strongly recommend you to install [IRuby notebook](https://github.com/minad/iruby) at the same time.
IRuby is the interactive Ruby environment and is useful to create plots quickly with Nyaplot.
The introduction video embeded on the top of this post was also created on IRuby.

IRuby depends on some software outside of Ruby-ecosystem, so its installation method is a bit complicated.
Please read [the description in README](https://github.com/domitry/nyaplot#install-iruby-notebook) to learn more.

## Usage
I already wrote tutorials, so I'd like to limit the role of this paragraph to the introduction of the basic usage.
You can skip reading this paragraph and find details on [Tutorial on nbviewer](http://nbviewer.ipython.org/github/domitry/nyaplot/blob/master/examples/notebook/Index.ipynb) and [Documents](http://rubydoc.info/gems/nyaplot/0.1.1/frames).

The minimum code to create Scatter plot is as shown below.

```ruby
plot = Nyaplot::Plot.new
sc = plot.add(:scatter, [0,1,2,3,4], [-1,2,-3,4,-5])
```

`Nyaplot::Plot` is the base class to create plots and `Nyaplot::Plot#add` is the method to add diagram to its instance.
The first argument is to specify the type of diagram, and the second and third are for data mapped into x and y axis.
`Plot#add` returns an instance of `Nyaplot::Diagram` and you can change the status like color and stroke of each diagram.

For example you can change its color by running the code below.
```ruby
color = Nyaplot::Colors.qual
sc.color(color)
```

`Nyaplot::Colors` is the simple wrapper for [Colorbrewer](http://colorbrewer2.org), and one of its methods `Nyaplot::Colors.qual` randomly returns colorset suitable for qualitative data.
If you excute these code on IRuby notebook, you can check if you favor the colorset through html-table based interface.
See [the tutorial](http://nbviewer.ipython.org/github/domitry/nyaplot/blob/master/examples/notebook/Colors.ipynb) to learn more.

![Nyaplot::Colors](https://dl.dropboxusercontent.com/u/47978121/gsoc/colors.png)

The plot generated from the code can be exported with two lines below.
```ruby
plot.show # show plot on IRuby notebook
plot.export_html # export plot to the current directory as a html file
```

We cannot explain Nyaplot without `Nyaplot::DataFrame`.
Example code above do not contain the word 'DataFrame', but the software interanally create an instance of `Nyaplot:DataFrame` and create plots based on it.

To create the same scatter plot as above, run the code below.

```ruby
df = Nyaplot::DataFrame.new({a: [0,1,2,3,4], b: [-1,2,-3,4,-5]})
plot.add_with_df(df, :scatter, :a, :b)
```

You may not feel any advantage of using `Plot#add_with_df` instead of `Plot#add`, but the former is useful when creating more complicated plots.
Have a look at [the tutorial](http://nbviewer.ipython.org/github/domitry/Nyaplot/blob/master/examples/notebook/Interaction_with_DataFrame.ipynb) to learn more.

## Conclusion
My GSoC term was over, but that do not mean the end of development.
There are still many things to do for improvement of this project.
For example Mapnya and Bionya are both experimental implementaion, and the function of Nyaplot::DataFrame is limited.
If you are interested in this project, please fork two repositories below and send me pull-requests.

* [Nyaplot on GitHub](https://github.com/domitry/nyaplot)
* [Nyaplotjs on GitHub](https://github.com/domitry/Nyaplotjs)

Any form of contribution is welcome.

At last I'd like to express my gratitude to my mentor Pjotr Prins and other members of SciRuby community.
I was not able to finish my GSoC term without great help from them.
Feedback from people outside of the community also helped me a lot.
I had a great summer thanks to all people related to this project :)
