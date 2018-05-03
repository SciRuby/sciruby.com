---
layout: post
title: "GSoC 2018 : Business Intelligence with daru"
date: 2018-05-03 10:23
comments: true
author: Rohit Ner
categories: [GSOC 2018, GSOC, Daru, Business, Intelligence]
---

#### What is Business Intelligence?

Business Intelligence is nothing but a way how business firms get smarter, smarter by analyzing information which concerns them. *“If you torture the data long enough, Nature will confess,”* said 1991 Nobel-winning economist Ronald Coase. BI helps easily achieve this lofty goal. Companies design strategies by figuring out bottlenecks from the data they have using BI.

#### BI with Ruby

Currently, there are many languages which provide excellent support for BI tools. [Sources](https://www.kdnuggets.com/2014/08/four-main-languages-analytics-data-mining-data-science.html) suggest a decline in usage of Ruby for BI. It is sad to see that in spite of having [daru](https://github.com/SciRuby/daru) which is an excellent library to handle big data by an easy and intuitive process, there is a lack of BI module.

#### My project


![2](https://rohitner.github.io/img/image_7_4.png){:.img-responsive}

This summer, I have been given a great opportunity to make Ruby more usable for BI. I will be working with the [**Ruby Science Foundation**](http://sciruby.com/) in this edition of GSoC!

We live in a world where people *Google* before they shop. If customers can’t avail your business information instantly over the internet and via a convincing medium like a website, they are sure will not trust your business to spend money on. The sites act as a source of data for the firm and hence a way to gain BI. 

My project involves developing daru-based load-analyze-process-visualise data tools for deriving Business Intelligence out of log data of web frameworks.

![3](https://rohitner.github.io/img/image_7_1.svg){:.img-responsive}

I plan to accomplish four milestones by the end of summer which are as follows.

* **Log Parsing** : Log management and analysis is essential for smooth operation of a system. I plan to make daru more usable for this purpose. The task consists of importing parsed log data from a log-analyzing-gem to daru and providing important metrics in the form of data/visuals as per requirements.

* **Data Cleaning** : A data cleaning module is one of the important things missing in the daru-based ecosystem. I plan to make this module akin to its counterparts in other languages along with additions from pre-existing data cleaning gems.

* **Data Analysis** : Methods used for getting a statistical insight of the data are scattered in daru. I plan to relocate all these methods in a single module along with useful methods for sampling and clustering.

* **Dashboard App** : To exhibit the abilities of daru’s infrastructure, it is necessary to provide an end product to the users. The dashboard app will be a demonstration of the combined result of all the other modules.

#### Next action

The first task as described above will be parsing logs. In the upcoming weeks, I will be adding support to daru for processing logs of standard web frameworks like Rails and Apache. I will be giving more details in the next blog.

Checkout my [proposal](https://docs.google.com/document/d/15wVNRAz8dwBdlCXYkOFwPfCzuxVr9nZwCpg2fOhU6ek/edit) for further details.
