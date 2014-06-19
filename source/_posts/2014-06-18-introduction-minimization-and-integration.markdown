---
layout: post
title: "Introduction: Minimization and Integration (Lahiru)"
date: 2014-06-18 16:45
author: Lahiru Lasandun
comments: true
categories: [GSOC2014,GSOC,Minimization,Integration]
---
<p class="note"><strong>Editor's Note:</strong> We have two students
working on numerical minimization and integration this summer, Rajat and
Lahiru. Rajat's introductory post appeared two weeks ago.</p>

Introduction 
--------------------------------------------------------
I'm Lahiru Lasandun and I'm an undergraduate of University of Moratuwa, Sri Lanka. I've been selected for
Google Summer of Code 2014 for SciRuby's Minimization and Integration projects.

I was working with SciRuby about a month before GSOC started and did some tests on how to enhance the performance of
these numerical computations. My first idea was to use multi-threading. With the instuctions and guidance of mentors, I
tested more methods such as Erlang multi-processing, the AKKA package of multi-threading, and finally OpenCL. The final
decision was to use OpenCL to enhance computation power of these mathematical computations with the support of multi-cores
and GPUs.

Minimization Gem
----------------
After GSOC started, I began working on SciRuby's Minimization gem. I proposed multidimensional minimization methods for the
Minimization gem, which already had plenty of unidimensional minimization methods. I chose two non-gradient 
and two gradient minimization methods as well as simulated annealing.

Integration Gem
---------------
For Integration, I proposed to replicate some unidimensional integration methods from the GNU Scientific Library, GSL. Additionally, I proposed to add OpenCL support to enhance performance of integration methods.

Current Progress
----------------
Currently, I am working on Nelder&ndash;Mead multidimensional minimization
method which is a non-gradient method, including working on the relevant
test cases.
