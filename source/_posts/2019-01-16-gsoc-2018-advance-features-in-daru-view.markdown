---
layout: post
title: "GSoC 2018 : Implementing advance features in daru-view"
date: 2019-01-16 17:30
comments: true
author: Prakriti Gupta
categories: [GSoC, GSoC 2018, daru, daru-view, Data Visualization, IRuby, HighCharts, Google Charts, DataTables, Nyaplot, Ruby]
---

This is a wrap of my magnificent adventurous journey of GSoC with SciRuby and I feel proud that I managed to contribute a significant amount to the development and progress of the project [daru-view](https://github.com/SciRuby/daru-view). This post summarizes my work in this period.


Daru-view now presents data in some more visualizations like HighMap and HighStock along with the already implemented HighCharts, GoogleCharts, DataTables and Nyaplot. It provides some more new cool features like formatting Daru::View::Table (GoogleCharts table) with different colors, pattern, etc., exporting charts to different formats, comparing different visualizations in a row and many more. Follow up with these [IRuby examples](https://github.com/SciRuby/daru-view/tree/master/spec/dummy_iruby) to know the current features equipped in daru-view. 

![HighStock](https://camo.githubusercontent.com/a0a98819e3421378873a96893502e044ceb09e84/68747470733a2f2f33327465657468676c69747465722e66696c65732e776f726470726573732e636f6d2f323031382f30352f68732e706e67 "Fig. 1: HighStock")
![HighMap](https://camo.githubusercontent.com/7e99eb68f3901995887d93160f2ef26756b531b5/68747470733a2f2f33327465657468676c69747465722e66696c65732e776f726470726573732e636f6d2f323031382f30352f686d5f696e6469612e706e67 "Fig. 2: HighMap")
![CSS styling](https://camo.githubusercontent.com/f2d3a7dc5889ec63028cc4d4faeb63c47ba5b270/68747470733a2f2f33327465657468676c69747465722e66696c65732e776f726470726573732e636f6d2f323031382f30352f6373732e706e67 "Fig. 3: CSS styling in HighCharts")
![Data from spreadsheet](https://camo.githubusercontent.com/c6145397f1bf1756bb2f2921ce35ea0d88b51099/68747470733a2f2f33327465657468676c69747465722e66696c65732e776f726470726573732e636f6d2f323031382f30362f696d706f72745f73707265616473686565742e706e67 "Fig. 4: Importing Data from Google spreadsheet")
![Multiple visualizations](https://camo.githubusercontent.com/5c9e5453dcf25cadceca7f558152bb0f75cdc737/68747470733a2f2f33327465657468676c69747465722e66696c65732e776f726470726573732e636f6d2f323031382f30372f6d756c7469706c655f67635f68632e706e67 "Fig. 5: Comparing data using different visualizations side by side")

These figures describe some of the features implemented in daru-view during GSoC.

# Application

The GSoC 2018 application can be found [here](https://docs.google.com/document/d/1id7ZJ4_rAEdXjg2yuBfcwSzV2QIiwpsfgc-gZdOioZ4/edit?usp=sharing).

# Code

GSoC 2018 work done summary – [Progress Report](https://github.com/SciRuby/daru-view/wiki/GSoC-2018---Progress-Report)
GSoC 2018 work presentation – [Advance Features in daru-view](https://docs.google.com/presentation/d/1lhf3QA5SmqA9YbMAjd6JnSJuZBuAfRKFBYs7JPKPRis/edit?usp=sharing)
Discourse Discussion – [daru-view discussion](https://discourse.ruby-data.org/t/gsoc-2108-project-advance-features-in-daru-view-discussion/43/50)

The work done during this GSoC has been explained in the following eight blog posts:

1. [GSoC 2018 Introduction – Goals defined](https://32teethglitter.wordpress.com/2018/05/01/gsoc-2018-introduction-advance-features-in-daru-view/)
2. [HighCharts and HighMaps](https://32teethglitter.wordpress.com/2018/05/20/gsoc-2018-coding-week-1/)
3. [Custom Styling CSS in HighCharts](https://32teethglitter.wordpress.com/2018/05/27/gsoc-2018-coding-week-2/)
4. [Exporing HighCharts | ChartWrapper | Importing data from google spreadsheet in Google Charts](https://32teethglitter.wordpress.com/2018/06/08/gsoc-2018-coding-week-3-4/)
5. [Exporting Google Charts | Charteditor](https://32teethglitter.wordpress.com/2018/06/24/gsoc-2018-coding-week-5-6/)
6. [Handling events in Google Charts | Multiple Charts in a row](https://32teethglitter.wordpress.com/2018/07/08/gsoc-2018-coding-week-7-8/)
7. [Formatters in Google Charts | Loading large set of data in DataTables](https://32teethglitter.wordpress.com/2018/07/24/gsoc-2018-coding-week-9-10/)
8. [Reduce a bunch of lines due to JS files in source HTML in rails | Rake task to add new adapter](https://32teethglitter.wordpress.com/2018/08/06/gsoc-2018-coding-week-11-12/)

# Future Work

The future work involves removing the dependency of daru-view on gems `google_visualr` and `lazy_high_charts` by creating our own gems. Check out these [new ideas](https://github.com/SciRuby/daru-view/wiki/Ideas#new-ideas-to-be-reviewed) that can be implemented in daru-view.

# FOSS

This has been my first attempt to explore the open source community. The summer was filled with the development of open source software and definitely was a great learning experience.

I really appreciate the effort by Google Open Source Committee for conducting GSoC every year. It is the
best platform for the aspiring programmers to improve their skill and give back to society by developing free
and open source software.

# Acknowledgements

I would like to express my sincere gratitude to Ruby Science Foundation, all the mentors and org admins for providing me this wonderful opportunity to enhance my knowledge and work independently on a project. I especially want to thank Shekhar for guiding me through the journey, helping and motivating me in every possible way.

I am very thankful to Google for organizing such an awesome program.
