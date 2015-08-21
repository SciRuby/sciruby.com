---
layout: post
title: "Summary of work on daru this summer for GSOC 2015"
date: 2015-08-16 23:53
comments: true
categories: 
author: Sameer Deshmukh
categories: [GSOC2015,GSOC, data, daru, analysis, visualization, munging]
---

Over this summer as a part of [Google Summer of Code 2015](www.google-melange.com), [daru](www.github.com/v0dro/daru/) received a lot of upgrades and new features which have it made a pretty robust tool for data analysis in pure Ruby. Of course, a lot of work still remains for bringing daru at par with the other data analysis solutions on offer today, but I feel the work done this summer has put daru on that path.

The new features led to the inclusion of daru in many of SciRuby's gems, which use daru's data storage, access and indexing features for storing and carrying around data. [Statsample](https://github.com/SciRuby/statsample), [statsample-glm](https://github.com/SciRuby/statsample-glm), [statsample-timeseries](https://github.com/SciRuby/statsample-timeseries), [statsample-bivariate-extensions](https://github.com/SciRuby/statsample-bivariate-extension) are all now compatible with daru and use `Daru::Vector` and `Daru::DataFrame` as their primary data structures. I also overhauled Daru's [plotting functionality](http://nbviewer.ipython.org/github/SciRuby/sciruby-notebooks/blob/master/Visualization/Visualizing%20data%20with%20daru%20DataFrame.ipynb), that interfaced with [nyaplot](https://github.com/domitry/nyaplot) for creating interactive plots directly from the data.

Also, new gems developed by other GSOC students, notably [Ivan's GnuplotRB gem](https://github.com/dilcom/gnuplotrb) and [Alexej's mixed_models gem](https://github.com/agisga/mixed_models) both now accept data from daru data structures. Do see their repo pages for seeing interesting ways of using daru.

The work on daru is also proving to be quite useful for other people, which led to a talk/presentation at [DeccanRubyConf 2015](http://www.deccanrubyconf.org/), which is one of the three major Ruby conferences in India. You can see the slides and notebooks presented at the talk [here](https://github.com/v0dro/talks/tree/master/DeccanRubyConf15). Given the current interest in data analysis and the need for a viable solution in Ruby, I plan to take daru much further. Keep watching the repo for interesting updates :)

In the rest of this post I'll elaborate on all the work done this summer.

## Pre-mid term submissions

Daru as a gem before GSOC was not exactly user friendly. There were many cases, particularly the iterators, that required some thinking before anybody used them. This is against the design philosophy of daru, or even Ruby general, where surprising programmers with ubiqtuos constructs is usually frowned down upon by the community. So the first thing that I did mainly concerned overhauling the daru's many iterators for both `Vector` and `DataFrame`.

For example, the `#map` iterator from `Enumerable` returns an `Array` no matter object you call it on. This was not the case before, where `#map` would a `Daru::Vector` or `Daru::DataFrame`. This behaviour was changed, and now `#map` returns an `Array`. If you want a `Vector` or a `DataFrame` of the modified values, you should call `#recode` on `Vector` or `DataFrame`.

Each of these iterators also accepts an optional argument, `:row` or `:vector`, which will define the axis over which iteration is supposed to be carried out. So now there are the `#each`, `#map`, `#map!`, `#recode`, `#recode!`, `#collect`, `#collect_matrix`, `#all?`, `#any?`, `#keep_vector_if` and `#keep_row_if`. To iterate over elements along with their respective indexes (or labels), you can likewise use `#each_row_with_index`, `#each_vector_with_index`, `#map_rows_with_index`, `#map_vector_with_index`, `#collect_rows_with_index`, `#collect_vector_with_index` or `#each_index`. I urge you to go over the docs of each of these methods to utilize the full power of daru.

Apart from the improvements to iterators there was also quite a bit of refactoring involved for many methods (courtesy [Alexej](https://github.com/agisga)). The refactoring of certain core methods has made daru much faster than previous versions.

The next (major) thing to do was making daru compatible with Statsample. This was very essential since statsample is very important tool for statistics in Ruby and it was using its own `Vector` and `Dataset` classes, which weren't very robust as computation tools and very difficult to use when it came to cleaning or munging data. So I replaced statsample's Vector and Dataset clases with `Daru::Vector` and `Daru::DataFrame`. It involved a significant amount of work on both statsample and daru &mdash; Statsample because many constructs had to be changed to make them compatible with daru, and daru because there was a lot of essential functionality in these classes that had to be ported to daru.

Porting code from statsample to daru improved daru significantly. There were a whole of statistics methods in statsample that were imported into daru and you can now use all them from daru. Statsample also works well with [rubyvis](https://github.com/clbustos/rubyvis), a great tool for visualization. [You can now do that with daru as well](https://github.com/SciRuby/statsample#visualizations).

Many new methods for reading and writing data to and from files were also added to daru. You can now read and write data to and from CSV, Excel, plain text files or even SQL databases.

In effect, daru is now completely compatible with Statsample (and all the other Statsample extensions). You can use daru data structures for storing data and pass them to statsample for performing computations. The biggest advantage of this approach is that the analysed data can be passed around to other scientific Ruby libraries (some of which listed above) that use daru as well. Since daru offers in-built functions to better 'see' your data, better visualization is possible.

See these [blogs](https://github.com/v0dro/daru#blog-posts) and [notebooks](https://github.com/v0dro/daru#notebooks) for a complete overview of daru's new features.

Also see the [notebooks in the statsample README](https://github.com/SciRuby/statsample#notebooks) for using daru with statsample.

## Post-mid term submissions

Most of time post the mid term submissions was spent in implementing the time series functions for daru.

I implemented a new index, the DateTimeIndex, which can used for indexing data on time stamps. It enables users to query data based on time stamps. Time stamps can either be specified with precise Ruby DateTime objects or can be specified as strings, which will lead to retrival of all the data falling under that time. For example specifying '2012' returns all data that falls in the year 2012. See detailed usage of `DateTimeIndex` and `DateTime` in conjunction with other daru constructs [in the daru README](https://github.com/v0dro/daru/blob/master/README.md).

An essential utility in implementing `DateTimeIndex` was `DateOffset`, which is a new set of classes that offsets dates based on certain rules or business logic. It can advance or lag a Ruby `DateTime` to the nearest day, or any day of the week, or the end or beginning of the month, etc. `DateOffset` is an essential part of `DateTimeIndex` and can also be used as a stand-alone utility for advancing/lagging `DateTime` objects. [This blog post](http://v0dro.github.io/blog/2015/07/27/date-offsets-in-daru/) elaborates more on the nuances of `DateOffset` and its usage.

The last thing done during the post mid term was complete compatibility with [Ankur Goel](https://github.com/AnkurGel)'s [statsample-timeseries](https://github.com/SciRuby/statsample-timeseries), which was created by  during GSOC 2013. Statsample-timeseries is a comprehensive suite offering various functions for statistical analysis of time sries data. It now works with daru containers and can be used for statistical analysis of data indexed on `Daru::DateTimeIndex`. See some use cases [in the README](https://github.com/SciRuby/statsample-timeseries/blob/master/README.rdoc).

I'd like to conclude by thanking all the people directly and indirectly involved in making this project a success - My mentor [Carlos](https://github.com/agarie) for his help and support throughout the summer, [Ivan](https://github.com/dilcom), [Alexej](https://github.com/agisga) and [Will](https://github.com/wlevine) for their support and feedback in various stages of developing daru. Also a big thank you to all the [SciRuby maintainers](https://github.com/orgs/SciRuby/teams) for making this happen!
