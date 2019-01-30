---
layout: post 
title: "GSoC 2018: RubyPlot GR - A Scientific Plotting Library for Ruby Built on GR Framework"
date: 2019-01-23 18:00
comments: true 
author: Pranav Garg 
categories: [GSoC, GSoC 2018, RubyPlot, Data Visualization, Ruby, Matplotlib, GR Framework]
---

### 1. About

For my summer of code Project I decided to create a plotting library.

From scratch.

In Ruby.

### 2. Application

The GSoC 2018 application can be found [here](https://github.com/pgtgrly/Cairo_Graph/wiki/Google-Summer-of-Code-2018-Application).

### 3. Code

The code for the project can be found [here](https://github.com/pgtgrly/GRruby-extension). 

RubyPlot is currently being developed [here](https://github.com/SciRuby/rubyplot).

### 3. The Plotting Architecture

The plotting architecture for the library was inspired by Late Dr John Hunter's Python Plotting Library "Matplotlib".

The Matplotlib Architecture is be broadly divided into three layers  (as shown in the masterpiece of a figure which I made below). The  Backend, The Artist and the scripting layer.

The Backend layer can further be divided into three parts : The Figure Canvas, The Renderer and the Event.

Matplotlib architecture is mostly written in Python with some of its  backend (the renderer AGG and Figure canvas ) written in C++ ( The  original AGG backend and helper scripts, which is quite tightly bound to  python). But the recently the backends are also written in Python using  the renderers which have Python APIs. The finer details of the Architecture can be found [here](https://aosabook.org/en/matplotlib.html). 

In interest of the time I decided to go for a iterative process to develop the Library. I decided to use an existing Artist layer. After a lot of discussion we decided to use GR Framework for the same. The only issue was that GR did not have a Ruby API. 

### 4. Creating C extensions for GR Framework:

To create the C extensions I initially decided to use Fiddle followed by FFi. But this lead to certain issues when it came to handling arrays. Hence I decided to go with the old fashioned Ruby C API to create extensions. The code for the same can be found [here](https://github.com/pgtgrly/GRruby-extension/tree/master/ext/grruby).

### 5. Creating Scripting Layer

The Scripting Layer Is meant for high level plotting. The scripting Library created on the GR Framework wrapper has the can plot the following:

* ScatterPlots

* Line graphs

* Bar Plots

* Stacked Bar plot

* Stacked Bar plot (Stacked along z axis)

* Candlestick plots

  All the above plots have a lot of customisation options, that can be looked into in the documentation.

Each Figure can have multiple subplots, each subplot can have multiple plots

### 6. Working of the Library

Here is how the library works.

![Imgur](https://i.imgur.com/sdNg7av.png)

Figure is the class that a user instantiates this is where all the  plotting take place. An instance contains the state of the figure. GR  framework is used as the artist layer which does all the plotting on the  figure. GR is also the backend.

GR artist layer functions are implemented in C language, we wrap the  functions to ruby classes which have the call method which executes the  GR function when the Object of the ruby class is called.
 Each of these ruby classes are called tasks which represents that they  perform a task, for example ClearWorkspace performs the task of cleaning  the workspace.

Now, the figure is divided into subplots. It is Subplot(1,1,1) by  default. So, figure has objects of subplot, each subplot is of type bar  plot or line plot etc. These plots are defined in the Plots module which  is submodule of Scripting module, the Plots module has a submodule  named BasePlots which defines the two bases of plots, LazyBase and  RobustBase.
 Lazy base is for plots which are dependent on state of the figure, for  example a bar graph depends on the location of axes. Every lazy plot has  a unique call function rather than inheriting it from LazyBase. In  LazyPlots the instances of GR Function Classes are called as soon as  they are instantiated. This all is done in the call function.
 Robust base is for plots which are which are independent of the state of  the Figure. For example: A scatter plot is independent of the location  of axes. Plots which are Sub classes of RobustBase append the instances  of GR function classes to tasks when initialized. These instances are  called via the call method defined in RobustBase.

So, each subplot which is of type bar plot or scatter plots etc.  inherits a base. Now, each subplot is just a collection of some tasks,  so it has a task list which stores the tasks to be performed i.e. the  Task objects, for example Scatter plot has tasks SetMarkerColorIndex  which sets the color of the marker, SetMarkerSize which sets the size of  the marker, SetMarkerType which sets the type of the marker and  Polymarker which marks the marker of defined color, size and style.
 Whenever a new Subplot object is initialized, for example  subplot(r,c,i), the figure is divided into a matrix with r rows and c  columns and the subplot initialized with index i is set as the active  subplot ans this active subplot is pushed into the subplot list. Each  subplot object has a unique identity (r,c,i) so if the user wants to  access a subplot which is already declared, this identity will be used.  When the subplot object is called (i.e. to view or save), it first  executes some necessary tasks and then pushes the tasks related to bar  plot, scatter plot, etc. to the task list.

Figure is a collection of such subplots and so Figure has a subplot list which stores the subplot objects.

![Imgur](https://i.imgur.com/H2vEO1i.png)

These tasks are just stored in the lists and are not performed (i.e. called) until the user asks to view or save the figure i.e. when the user calls view or save (which are tasks themselves) the tasks are performed (i.e. called) and the figure is plotted. This is done by using the Module Plotspace.  
When the figure calls the task view or save, these tasks call the Plotspace Object and the state of figure is copied to the Plotspace Object and this Object starts executing( or performing) i.e. calling tasks from task list of each subplot in subplot list and the figure is plotted and viewed or saved.  

Here is the current view of Library:

![imgur](https://i.imgur.com/0HwzAtG.png)

### Future

The Library is currently being developed by SciRuby community [here](https://github.com/SciRuby/rubyplot). Currently, it is a static library, after further development, it's Architecture should look like the following:

![imgur](https://i.imgur.com/AdfPQlT.png)



### Acknowledgements

I would like to thank Sameer Deshmukh and Prasun Anand for guiding me through every step of software design and helping me out through every decision. I would also like to thank Dr John Woods and Dr Pjotr Prins for their valuable feedback. I am glad to be a part of SciRuby community and I hope to further contribute towards it's goal.

I would also like to thank Arafat Khan, a fellow GSoCer who worked on the library using Rmagick as the backend renderer for our fruitful debates over the architecture of the library

Finally, I would like to thank Google for giving me this opportunity.