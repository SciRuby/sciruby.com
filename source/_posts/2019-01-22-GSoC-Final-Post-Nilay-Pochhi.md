---
layout: post
title: "GSoC Final Post, Nilay Pochhi"
date: 2019-01-22 17:00
comments: true
author: Nilay Pochhi
---

The GSoC 2018 coding period has ended. This post summarizes my journey throughout the period.

<!--more-->

### GSoC Proposal

You can find my GSoC proposal [here](https://docs.google.com/document/d/1_gUCa1LNPZmkKkqCEXR7eJu-IZ9DiiA-n0kG660hLIE/edit?usp=sharing) and the link to the project repository [here](https://github.com/sciruby/networkx.rb).

### What is this thing?

NetworkX.rb is a graph analysis library for Ruby. It is named after the popular Python package named NetworkX.

### Why in the world do we need this library?

You can find graphs everywhere. From studying molecular biology to studying the way internet functions, graphs find a place in our everyday lives. A ubiquitous example is the one of Google Search. PageRank Algorithm, which governs the importance given to a particular webpage and is widely used in the Google Search, is a graph based algorithm. Analysis of the graphs is of prime importance and there is heavy research going on in the scientific community on the said topic.
Ruby lacked a proper graph analysis library and hence to fill the vacuum, we decided to make a library of our own.

### Architecture of the library

There are four main classes; Graph, DiGraph, MultiGraph and MultiDiGraph. DiGraph inherits from Graph, MultiGraph inherits from Graph and MultiDiGraph inherits from DiGraph. The classes have various graph modification methods implemented. To get a detailed look at the methods, please check my GSoC proposal.
Several graph analysis algorithms have been implemented. These algorithms are graph type agnostic i.e. the implementation handles all cases of graphs. Some algorithms however, have support for only a subset of types because they simply aren't defined for other types.

### Finished, unfinished and future goals

Most of the goals that were proposed have been reached. NetworkX.rb supports following algorithms:

* Flow
    * Capacity Scaling
    * Edmonds Karp
    * Shortest Augmenting Path
    * Preflow push
* Auxillary Algorithms
    * Clique finding
    * Cycle finding
    * DAG algorithms
    * Eccentricity finding
    * Maximal independent set
    * Minimum Spanning Tree
    * Vitality finding
    * Wiener index
* Link Analysis
    * Pagerank
    * HITS
* Shortest path
    * Dijkstra
    * Floyd Warshall
    * Bellman Ford
    * Johnson's algorithm
    * Shortest paths in unweighted graphs
* Traversals
    * BFS
    * DFS
    * Edge-DFS
* Operators
    * Union of two/more graphs
    * Product of two/more graphs
    * Difference of two/more graphs
    * Symmetric difference of two/more graphs

The GSoC goals though reached, are missing many algorithms like assortativity finding etc. which are important for analysis of complex networks. I'm also thinking of including a visualization/interactive tool which may be useful for community as it would make the library language agnostic i.e. one would not require the knowledge of Ruby language to model networks and perfom analysis on it.

### Acknowledgements

I would like to express my sincere gratitude to my mentor, Athitya, who answered my every question patiently and gave me ample opportunities to perform the job and encouraged, even when I was lagging behind in my GSoC timeline.
I'd like to thank the entire SciRuby community for giving me this opportunity and exposing me to the world of Opensource development.
I'd like to keep contributing to SciRuby community and introduce first timers to the world of FOSS.