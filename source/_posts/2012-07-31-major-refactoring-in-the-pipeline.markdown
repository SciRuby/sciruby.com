---
layout: post
title: "Major Refactoring in the Pipeline"
date: 2012-07-31 16:23
comments: true
author: John Woods
categories: [NMatrix]
---
Activity has picked up enormously on [our listserv](http://groups.google.com/group/sciruby-dev) over the last few weeks,
which makes us all very happy.

[Chris Wailes](https://github.com/chriswailes) and I have been working on a major refactoring of [NMatrix](http://github.com/SciRuby/nmatrix),
our replacement for NArray. You may recall that we released our first alpha (0.0.1) back in April, and that we've been
relatively silent since then.

We're pleased to announce that our second alpha, NMatrix v0.0.2, should be available as a gem by mid-August.

Our first alpha was a proof of concept. The second alpha is the framework upon which we will build an empire of chunky
bacon.

Chris came up with the idea to convert all of NMatrix's Ruby/C template code into C++ templates, and we've been working
on that steadily. These are cleaner and easier to maintain than the C parser-based templates I wrote a few months ago.

In addition, I have incorporated basic slicing, contributed by [Aleksey Timin](http://github.com/flipback), who has been
absolutely essential to our growing project.

One major hiccup has been that NMatrix and NArray conflict with one another when both are included (particularly poor
planning on my part). This means that Ruby/GSL can't work with NMatrix without some modifications -- which are underway
in a [fork](http://github.com/SciRuby/rb-gsl). We plan to release fixes for Ruby/GSL and Statsample as soon as possible.

We want to again thank [Brighter Planet](http://brighterplanet.com) for their support of our summer coding fellowship.
Little of this would be possible without them.