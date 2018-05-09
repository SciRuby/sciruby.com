---
layout: post
title: "GSoC Introductory Post, Nilay Pochhi"
date: 2018-04-30 17:00
comments: true
author: Nilay Pochhi
---

The day before the dreaded DBMS end semester exam, I got the news that my proposal NetworkX.rb was selected for GSoC 2018. My happiness knew no bounds and lets just say that the effect on my exams has not necessarily been a positive one!

<!--more-->

I'll be working for project `NetworkX.rb` which falls under the aegis of SciRuby organization. Primarily, my work would be to build a graph analysis library for Ruby. My mentors are [Sameer](https://github.com/v0dro/), [Athitya](https://github.com/athityakumar/) and [Mridul](https://github.com/MridulS). You can follow the project [here](https://github.com/athityakumar/networkx.rb) and possibly give us a star :P.

Ruby lacks a proper graph analysis library. Python has it in NetworkX. Using NetworkX as a guidance point, I decided to fill in the gap or atleast try to (since NetworkX is a pretty huge library) during this GSoC period. A proper complex network analysis library would be a good addition to the arsenal.

### Journey before GSoC Acceptance

Before GSoC, my mentors (Athitya, Sameer and Mridul) and I decided on the data structures we were going to use. I implemented a basic graph class which implemented methods like `add_edge`, `add_node` etc. The plan is to extend the class to include basic digraphs, multigraphs and dimultigraphs. I have enumerated the methods I'd like to implement in the proposal submitted. I wrote tests using `rspec` for the graph class and have used `rubocop` as a style checking tool. For algorithms requiring heavy computation we're planning to use `nmatrix` library which has its backend in C++ and hence would provide some performance upgrades.

### Journey during GSoC

During GSoC I plan to implement the remaining classes and the algorithms required for graph analysis. The algorithms to be implemented during the GSoC period are -

* Basic Traversals
* Shortest Path Algorithms
* Network Flow Algorithms
* Set operations on graphs
* Link Analysis Algorithms such as PageRank and HITS
* Some more auxillary algorithms

For those who want to get into more implementation level details and further details on my weekly plan may check my proposal [here](https://docs.google.com/document/d/1_gUCa1LNPZmkKkqCEXR7eJu-IZ9DiiA-n0kG660hLIE/edit?usp=sharing). The proposal may change during community bonding period as the discussions with my mentors progress.

I'll post the details of my updates weekly and I'm hoping to have an amazing summer working on this project.  
