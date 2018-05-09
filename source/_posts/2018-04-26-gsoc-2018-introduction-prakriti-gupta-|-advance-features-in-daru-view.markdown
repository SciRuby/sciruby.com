---
layout: post
title: "GSoC 2018 Introduction | Advance features in daru-view"
date: 2018-04-26 15:25
comments: true
author: Prakriti Gupta
---

## Introduction to the Project - Daru-view

Leaving no stone unturned was my level of enthusiasm when I started preparing for [GSoC](https://summerofcode.withgoogle.com/). Keeping the same spirit going, I will be working with [Ruby Science Foundation](http://sciruby.com/) for GSoC 2018 under the guidance of my mentors [Shekhar Rajak](https://github.com/Shekharrajak) and [Athitya Kumar](https://github.com/athityakumar).

[Daru-view](https://github.com/SciRuby/daru-view) is a gem created for the purpose of data visualisation in ruby. It uses plotting tools like [GoogleCharts](https://developers.google.com/chart/interactive/docs/gallery), [HighCharts](https://www.highcharts.com/demo), [NyaPlot](https://github.com/SciRuby/nyaplot?files=1) and [DataTables](https://datatables.net/examples/index) for plotting various interactive charts and tables. It works both with web frameworks of ruby like rails, sinatra, nanoc and with iruby notebook. Have a look at some of the [examples](http://nbviewer.jupyter.org/github/sciruby/daru-view/tree/master/spec/dummy_iruby/), they’re really cool! :)

However, the indirect access of these plotting tools through some dependent gems limits their usage. So, my project focuses on the implementation of various advance features in daru-view which includes extending the code of these dependent gems and the execution of various features available for Google charts JS, HighCharts and DataTables.

## Journey to GSoC

GSoC 2018 was an advent in my life as Hogwarts would be in a Wizard’s life. If we are comparing it with magic then as in magic things must come out of a Wand, right, the case been the same. Merging ruby with science was none less than a magic spell and I felt like

![1](https://github.com/Prakriti-nith/Daru-view-examples/blob/master/images/harry.gif)

I started with the beginner-friendly issues to understand the workflow of the project. Soon after, I found myself working on with test cases as they held high priority at that time. Working on bug fixes and further feature addition gave me a deep insight of the project. All this wouldn’t have been possible without the coordination of my mentor. Thanks a ton to my mentor for being responsive and helping me out.

All of my contributions in daru-view can be tracked [here](https://github.com/SciRuby/daru-view/pulls?utf8=%E2%9C%93&q=is%3Apr+author%3APrakriti-nith+). Before the GSoC results were out, I managed to work on 9 pull requests, out of which 8 were merged.

## The Plan and expected outputs

Now, as I have entered into the Hogwarts :P , I plan to accomplish the following tasks during my way. My first lesson of the wizardy world would be on:

### HighCharts

HighCharts uses lazy_high_charts as the dependent gem to access the features of [HighCharts](https://www.highcharts.com/demo) JS. In the HighCharts section, I plan to work on some of the feature additions like:

* Implementation of two classes [HighStock](https://www.highcharts.com/stock/demo) and [HighMap](https://www.highcharts.com/maps/demo) which generates some really impressive plots.
* Custom styling CSS in HighCharts to make charts more interactive.

![2](https://github.com/Prakriti-nith/Daru-view-examples/blob/master/images/highcharts.png "CSS styling in Highcharts")

* Export the chart in various formats.
* I will also implement some advance HighCharts such as circular gauge chart.

Every wizard must know how to use the spell of:

### GoogleCharts

GoogleCharts uses GoogleVisualr as the dependent gem to access the features of [GoogleCharts](https://developers.google.com/chart/interactive/docs/gallery) JS. In this section, I will be working on some feature additions to make the charts and tables more interactive. These are:

* Implementation of [ChartWrapper](https://developers.google.com/chart/interactive/docs/drawing_charts#chartwrapper) and [ChartEditor](https://developers.google.com/chart/interactive/docs/reference#charteditor-class) classes which will provide the users more flexibility like they can edit the chart at runtime.
* Exporting the charts in various formats so that they can be easily downloaded by the use of code.
* Showing multiple charts simultaneously. This will provide the users with the facility to compare the data in a more interactive way.

![3](https://github.com/Prakriti-nith/Daru-view-examples/blob/master/images/googlecharts.png "Multiple charts in a row")

* Adding some more methods in Google DataTables to make it more user-friendly.
* Right now, I am working on the idea to import the data from google spreadsheets.

Now, Datatables as a part of defence against the dark arts class:

### DataTables

[DataTables](https://datatables.net/examples/index) uses daru-data_tables as the dependent gem. In DataTables I plan to work on some features like:

* Exporting the chart in HTML format and some code restructuring.
* Implementation of the server-side processing in the Datatables in order to load large amount of data, piece by piece.

Its the strength and determination that makes a wizard strong. Apart from these spells, I will put my effort in removing a bunch of lines at the source html file because of js and will work on completing the basic structure of the plugin to add a new adapter. I will also add numerous examples to demonstrate the capabilities of daru-view.

This project might involve a number of challenges but never letting my spirit down, I will be at my best effort to achieve them during the summer.

To get in touch and add more of your ideas, do visit this discourse discussion [topic](https://discourse.ruby-data.org/t/gsoc-2108-project-advance-features-in-daru-view-discussion/43).

All set to be a master wizard! ;)

![4](https://github.com/Prakriti-nith/Daru-view-examples/blob/master/images/dumbledore.gif)
