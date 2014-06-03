---
layout: post
title: "Introducing the FFTW SciRuby GSoC project"
date: 2014-05-31 22:23
author: Magdalen Berns
comments: true
categories: FFTW, GSoC
---
My name is Magdalen Berns and I am a physics student with a technical background in live audio. I am particularly interested in using science and technology to improve access for all.

This summer, I will be working on implementing the external library appropriately named "Fastest Fourier Transform in the West" version 3 (FFTW3) C and fortran API in ruby for this year's Google Summer of Code (GSoC).
The primary aim of the project idea is to give SciRuby the capability to handle signal analysis, processing and synthesis by performing discrete fast fourier transform operations on NMatrix objects.

After some investigation during the preparation stages of GSoC, it was determined that implementing FFTW3 is more desirable than starting from scratch in pure ruby tbecause the FFTW3 API is already extensively used, developed and optimised far beyond what would be achievable in just 3 months.
So, by putting FFTW3 in the driving seat, this allows the SciRuby project to take advantage on the good work of the FFTW3 developers by bringing it to ruby and should hopefully allow me to use SoC to concentrate on making the most of the operations that the API provides the project with over the summer.
Putting nmatrix to the test with the FFTW3 should give users the opportunity to test drive nmatrix and SciRuby's nmatrix developers, a chance to root out bugs to be fixed.

Since a gem called ruby-ffw3 already existed to perform FFTW3 operations on narray objects so far, I forked that repository as a starting point and [things are progressing on my fftw3 repository on github](https://github.com/thisMagpie/fftw3) right now. 

My mentor for this project is Colin Fuller who is an exceptionally talented programmer (and he really knows his git too) so he has been a great help so far as I adapt to the learning curve of working in C and ruby (languages which I am less familiar with than say, java or javascript).

Since Colin has already shared some really useful tips and tricks, it is likely that over the coming few weeks, as well as updating everyone following [Sciruby.com](http://sciruby.com)about the specifics of my project work.
I am going to try to share those useful gems of information I have gathered so far with [Sciruby.com](http://sciruby.com) followers, in addition to my weekly project updates since I tend to post handy instructions for reference on my site anyway.
So, I may as well share them where they are going to be read, on sciruby too so other's can hopefully benefit!

I have already posted a few useful bits and bobs on [thismagpie.com](http://thismagpie.com) which relate to my work so far.
I hope to add those to [Sciruby.com](http://sciruby.com) too provided the readers are interested in that and time permits.Of course, [Sciruby.com](http://sciruby.com) readers can feel free to have a browse of the keywords [sciruby](http://thismagpie.com/keyword/sciruby), [ruby](http://thismagpie.com/keyword/ruby) and [git](http://thismagpie.com/keyword/git) on there for the time being.
I sometimes add posts, manuals and tutorials from external sites where I find useful ones on the www too, so watch out for these too, if you think they might be of use to you as well.

Please, feel free to watch or follow along as the project comes together and those inclined are welcome to share constructive comments and advice or raise bugs on the fftw3 issue tracker. Input about my work is very welcome as the project progresses. This gem is being written for sciruby users, after all!

Follow on: Twitter (Facebook) or github: @thismagpie
