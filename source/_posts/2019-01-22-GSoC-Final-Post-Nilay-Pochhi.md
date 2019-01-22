---
layout: post
title: "GSoC Final Post, Nilay Pochhi"
date: 2019-01-22 17:00
comments: true
author: Nilay Pochhi
---

The GSoC 2018 coding period is about to end. This post summarizes my journey throughout the period.

<!--more-->

### GSoC Proposal

You can find my GSoC proposal [here](https://docs.google.com/document/d/1_gUCa1LNPZmkKkqCEXR7eJu-IZ9DiiA-n0kG660hLIE/edit?usp=sharing) and the link to the project repository [here](https://github.com/sciruby/networkx.rb).

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

There was one algorithm, Network Simplex, in my proposal which I haven't implemented. I plan to do it soon.

The GSoC goals though reached, are missing many algorithms like assortativity finding etc. which are important for analysis of complex networks. I'm also thinking of including a visualization/interative tool which may be useful for community as it would make the library language agnostic i.e. one would not require the knowledge of Ruby language to model networks and perfom analysis on it.

### Acknowledgements

I would like to express my sincere gratitude to my mentor, Athitya, who answered my every question patiently and gave me ample opportunities to perform the job and encouraged, even when I was lagging behind in my GSoC timeline.
I'd like to thank the entire SciRuby community for giving me this opportunity and exposing me to the world of Opensource development.
I'd like to keep contributing to SciRuby community and introduce first timers to the world of FOSS.