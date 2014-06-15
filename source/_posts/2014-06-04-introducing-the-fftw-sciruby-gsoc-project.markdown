---
layout: post
title: "Introducing the FFTW SciRuby GSoC project"
date: 2014-06-04 12:30
author: Magdalen Berns
comments: true
categories: [FFTW,GSOC,GSOC2014,NMatrix,Students]
---
My name is Magdalen Berns and I am a physics student with a technical
background in live audio. I am particularly interested in using science
and technology to improve access for all.

This summer, I will be working on implementing the external library
appropriately named "Fastest Fourier Transform in the West" version 3
(FFTW3) C and Fortran API in Ruby for this year's Google Summer of Code
(GSoC).

The primary aim of the project is to give SciRuby the capability to
handle signal analysis, processing and synthesis by performing discrete
fast Fourier transform operations on NMatrix objects.

After some investigation during the preparation stages of GSoC, it was
determined that implementing FFTW3 is more desirable than starting from
scratch in pure Ruby because the FFTW3 API is already extensively used,
developed, and optimised far beyond what would be achievable in just
three months. So, putting FFTW3 in the driving seat allows the SciRuby
project to take advantage of the good work of the FFTW3 developers by
bringing it to Ruby.

Putting NMatrix to the test with FFTW3 should give users the
opportunity to test drive NMatrix &mdash; and SciRuby's NMatrix developers a
chance to root out bugs.

Since a gem called ruby-fftw3 already existed to perform FFTW3 operations
on NArray objects, I forked that repository as a starting point. [Things are progressing on my Github fork](https://github.com/thisMagpie/fftw) right now. 

My mentor for this project is Colin Fuller who is an exceptionally
talented programmer &mdash; and he really knows his git too. He has been a
great help as I adapt to the learning curve of working in C and
Ruby (languages which I am less familiar with than say, Java or
JavaScript).

As I work, I intend to share useful gems of information I gather. Those, in addition to my weekly project updates, will appear right here in this blog so others can hopefully benefit.

I have already posted a few useful bits and bobs on
[thismagpie.com](http://thismagpie.com) which relate to my work so far.
I hope to add those to the SciRuby blog, too, provided
the readers are interested in that and time permits. Of course, readers here can feel free to have a browse
of the keywords [sciruby](http://thismagpie.com/keyword/sciruby),
[ruby](http://thismagpie.com/keyword/ruby) and
[git](http://thismagpie.com/keyword/git) on there for the time being.
I sometimes add posts, manuals and tutorials from external sites where I
find useful ones on the web too, so watch out for these too.

Please, feel free to watch or follow along as the project comes together
and those inclined are welcome to share constructive comments and advice
or raise bugs on the fftw3 issue tracker. Input about my work is very
welcome as the project progresses. This gem is being written for the
community, after all!

You can find me on Twitter (Facebook) or GitHub under the username @thisMagpie.
