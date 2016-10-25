---
layout: post
title:  "GSOC 2016 : Final Report (SpiceRub)"
date:   2016-08-23 00:00:35 +0530
categories: gsoc
---

Finally, GSOC 2016 has come to an end. This last blog post will aim to sum up the work done in these 
past few months and how the project can proceed to hit more milestones. Below are some links to my work :-

[Working Repository][repo] : GitHub Repo

[List of Commits][commits] : List of commits to the above repository

[Example iRuby Notebooks][note] : A bunch of iRuby Notebook examples of the Ruby API

Firstly, I must admit that I was unable to meet all the goals of my proposal. In retrospect, perhaps it was a bit too ambitious for my skill level at the time, and I undoubtedly spent a lot of time learning, about Ruby-C extensions, the SPICE Toolkit, and good Ruby code and API.

General software requirements such as a shippable gem with an install script that downloads the correct binary dependencies and a dataset fetcher were among the targets in my proposal that I could not meet. If you'd like to read the full proposal, you can have a look at it [here][prop].

With that said, I am happy with what I did complete, and along with most of the functions in the proposal list being ported successfuly, I have added 3 Ruby classes to provide a better abstracted experience while using the Ephemerides subsystem of SPICE. Given the vastness of SPICE itself, this seems like meagre coverage, but it is a stepping stone that I (and I hope others) would build upon.

## **The SpiceRub API**

Please follow the blog links below to read the posts concerning the Ruby classes, the posts basically demonstrate the simple API that can be used for various tasks involving ephemerides. It would help if you followed the order of the links below as they build on top of each other.

[KernelPool][pool] :- A Singleton class to handle the loading and unloading of SPICE Data (Kernels)

[Time][time] :- A class that references ephemeris time and has flexible construction functions

[Body][body] :- A class that represents a body in space whose motion can be observed with respect to another body.

I spent majority of the post mid-term period on the latter two classes, while most of the time before was spent on writing C extensions to port the SPICE API to Ruby. (and learning good  `spec` manners, something that was not that quick to grow on me but towards the end got ingrained into me)

I ran into a lot of bugs, learnt more about compiler flags and other building options, and it gave me surprisingly good exposure to low level language trivia for a high level Ruby project. Honestly, I can type `SpiceRub` faster than most words on my keyboard now =)

## **Future Road Map**

Things that I'll tackle (after a short break) include :-

1) Installation Integrity so that `gem install` does everything needed for installation.

2) Complete Documentation

3) The test coverage of SpiceRub::Time is currently lacking because I changed the API at the last minute, but as most of these functions wrap around Native functions that have already been tested, this won't be too high a priority, but it will be done.

4) Kernel Fetcher Script : Having to crawl the web for relevant Kernels was a massive headache during this project, and adding something that directly refers to NAIF's FTP servers would be a neat addition.
 
5) Expand the API : There is a lot of SPICE concept I am not well versed with, and there are a bunch of functions ported that would work very well with a better API. (Most immediate task I can see is making a better API for the Geometry Finder subsystem, of which many functions have already been ported in `/ext/spice_rub/spice_geometry.c`)

## **Acknowledgements**

I'd like to thank my mentors for this project, Dr. John Woods, Shaun Stewart, and Victor Shepelev for their guidance and knowledge. John and Shaun with their expertise in Astro research and Victor's vast knowledge about professional Ruby code has helped keep this whole ship together, and I'm looking forward to sailing it a few more journeys. 

Also, John made [NMatrix][nmatrix] which is sort of the backbone for a lot of SpiceRub's functionality.

Finally, I would like to thank Andrew Annex who wrote [SpicePy][spiceypy], and Philip Rasch who wrote [spiceminer][miner] , two Python wrappers for the SPICE Toolkit. SpicePy has a high port and test coverage (I lifted a lot of tests from here that weren't available in the SPICE documentation and SpiceMiner had an OOP style API which I referred to while designing the `Body` class.

It's been an incredible summer, thank you for reading :)



[nmatrix]: https://github.com/sciruby/nmatrix
[miner]: https://github.com/darasch/spiceminer
[spiceypy]: https://github.com/AndrewAnnex/SpiceyPy
[spicerub]: https://github.com/gau27/spice_rub
[readme]: https://github.com/gau27/spice_rub/blob/master/README.rdoc
[toolkit]: https://naif.jpl.nasa.gov/naif/toolkit_C.html
[time_paradox]: https://en.wikipedia.org/wiki/Year_2038_problem
[unitim]: https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/cspice/unitim_c.html
[str2et]: https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/cspice/str2et_c.html
[prop]: https://summerofcode.withgoogle.com/serve/5210336198131712/
[body]: http://gauravtamba.me/gsoc/2016/08/21/body-tutorial.html
[pool]: http://gauravtamba.me/gsoc/2016/06/11/week1_soc.html
[time]: http://gauravtamba.me/gsoc/2016/08/21/time-tutorial.html

[repo]: http://github.com/gau27/spice_rub
[commits]: https://github.com/gau27/spice_rub/commits/master
[note]: https://github.com/gau27/spice_rub/tree/master/examples