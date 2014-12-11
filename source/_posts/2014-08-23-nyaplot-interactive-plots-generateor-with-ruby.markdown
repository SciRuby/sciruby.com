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

Three months of GSoC 2014 term was over this week and finally I
released my product [Nyaplot as a gem](http://rubygems.org/gems/nyaplot). 
This blog post was written to introduce Nyaplot version 0.1.1.

## Features

There are various plotting libraries in the world as ggplot2 from R
community and [Plotrb](https://github.com/zuhao/plotrb) and
[Rubyvis](https://github.com/clbustos/rubyvis) from Ruby.  The
features of Nyaplot compared with those libraries are
**interactivity**, **simplicity**, and **extensibility**.

Interactivity is the main theme of my D3 project in GSoC 2014. You can
soon find the aforementioned interactivity by clicking plots with your
mouse or trackpad. This feature is also available on browsers bundled
with Android or iOS thanks to technologies like SVG and WebGL.

However, the word 'interactivity' is not limited to situations like
the above. You can explore that for yourself by using Nyaplot in [IRuby notebook](https://github.com/minad/iruby). Various 
modules prepared
by Nyaplot help you to create plots interactively in the notebook.
You can also publish the result quickly by uploading the notebook to
your Dropbox storage, a gist or pastebin, or somewhere else. [Here is an example](http://nbviewer.ipython.org/gist/mgiraldo/a68b53175ce5892531bc)
which Mauricio Giraldo created and published on gist.

Simplicity is also an important element.  Many plotting libraries has
MATLAB-like function-based API but Nyaplot does not.  Nyaplot is
designed in a more Ruby-like object-oriented style, and its plots
consist of various objects created from different classes. But Nyaplot
has simple shortcut methods and users may avoid the more complex API.

![Extension libraries](https://dl.dropboxusercontent.com/u/47978121/gsoc/extensions_for_blog_post.png)

Nyaplot can be easily and dramatically extended with some JavaScript
code. It bundles 3 extension libraries in its gem package: Nyaplot3D
for 3D plots, Mapnya for map visualization, and Bionya for circular
plots inspired by circos. Their structure is very simple and you can
write the extension easily if you have a little experience in
JavaScript and Ruby. For example, the Ruby part of Bionya has only
about a hundred lines, even though the graphic's style is far from
that of standard plots.

## Quick start

You can install Nyaplot by simply running `gem install nyaplot`.
After that, find example code from
`path_to_gems/nyaplot-0.1.1/examples/rb` and run some of them using
`ruby` command.  These scripts will generate some plots with Nyaplot
and export them as html files to current directory.

In addition, I strongly recommend you to install [IRuby notebook](https://github.com/minad/iruby) at 
the same time. IRuby is a
web-based, interactive Ruby environment and is useful for quickly
creating plots with Nyaplot. The introduction video embedded at the
top of this post was also created on IRuby.

IRuby depends on some software outside of Ruby-ecosystem, so its
installation method is a bit complicated. Please read [the description in the readme](https://github.com/domitry/nyaplot#install-iruby-notebook)
to learn more.

## Usage

I already wrote tutorials, so I'd like to limit the role of this
paragraph to the introduction of the basic usage. You can skip
reading this paragraph and find details in [the nbviewer tutorial](http://nbviewer.ipython.org/github/domitry/nyaplot/blob/master/examples/notebook/Index.ipynb)
and [documentation](http://rubydoc.info/gems/nyaplot/0.1.1/frames).

The minimum code to create a scatter plot is as shown below:

```ruby
plot = Nyaplot::Plot.new
sc = plot.add(:scatter, [0,1,2,3,4], [-1,2,-3,4,-5])
```

`Nyaplot::Plot` is the base class to create plots and
`Nyaplot::Plot#add` is the method to add diagram to its instance.  The
first argument is to specify the type of diagram, and the second and
third are for data mapped into _x_ and _y_ axes.  `Plot#add` returns an
instance of `Nyaplot::Diagram` and you can change attributes like
color and stroke of each diagram component.

For example you can change its color by running the code below.
```ruby
color = Nyaplot::Colors.qual
sc.color(color)
```

`Nyaplot::Colors` is the simple wrapper for
[Colorbrewer](http://colorbrewer2.org), and one of its methods
`Nyaplot::Colors.qual` randomly returns colorset suitable for
qualitative data.

If you execute this code in IRuby notebook, you can check if you favor
the colorset through html-table based interface. [See the tutorial to learn more.](http://nbviewer.ipython.org/github/domitry/nyaplot/blob/master/examples/notebook/Colors.ipynb)

![Nyaplot::Colors](https://dl.dropboxusercontent.com/u/47978121/gsoc/colors.png)

The plot generated from the code can be exported with two lines below.

```ruby
plot.show # show plot on IRuby notebook
plot.export_html # export plot to the current directory as a html file
```

We cannot explain Nyaplot without `Nyaplot::DataFrame`. Example code
above does not contain the word 'DataFrame', but the software
internally create an instance of `Nyaplot:DataFrame` and creates plots
based on it.

To create the same scatter plot as above, run the code below.

```ruby
df = Nyaplot::DataFrame.new({a: [0,1,2,3,4], b: [-1,2,-3,4,-5]})
plot.add_with_df(df, :scatter, :a, :b)
```

You may not feel any advantage of using `Plot#add_with_df` instead of `Plot#add`, but the former is useful when creating more complicated plots. [Have a look at the tutorial to learn more.](http://nbviewer.ipython.org/github/domitry/Nyaplot/blob/master/examples/notebook/Interaction_with_DataFrame.ipynb)

## Conclusion

My GSoC term has concluded, but that do not mean the end of
development.  There are still many things to do for improvement of
this project.  For example Mapnya and Bionya are both experimental
implementation, and the functionality of Nyaplot::DataFrame is
relatively limited.  If you are interested in this project, please
fork either or both of the two repositories below and send me
pull-requests.

* [Nyaplot on GitHub](https://github.com/domitry/nyaplot)
* [Nyaplotjs on GitHub](https://github.com/domitry/Nyaplotjs)

Any form of contribution is welcome.

At last I'd like to express my gratitude to my mentor Pjotr Prins and
other members of SciRuby community.  I would not have been able to
finish my GSoC term without great help from them.  Feedback from
people outside of the community also helped me a lot.  I had a great
summer thanks to all people related to this project.
