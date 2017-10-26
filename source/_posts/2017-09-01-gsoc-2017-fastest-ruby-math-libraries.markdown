---
layout: post
title: "GSoC 2017 : Creating the fastest Math libraries for Ruby by using the GPU through OpenCL, CUDA and ArrayFire."
date: 2017-09-15 22:46
comments: true
author: Prasun Anand
categories: [GSoC,GSoC 2017, GPU, GPGPU, ArrayFire, HPC, CUDA, OpenCL, High Performance Computing, Ruby]
---

GSoC 2017 is about to end. This post summarises my work during the course of summer.


ArrayFire-rb now supports linear algebra on GPU and CPU. Currently only double dtype has been implemented.
It supports dense and sparse matrices. It has multiple backends namely, CUDA, OpenCL and CPU.

![Matrix Addition](https://github.com/prasunanand/gsoc_blog/blob/master/img/sciruby_blog/arrayfire/add.png?raw=true "Fig. 1: Matrix Addition")
![Matrix Subtraction](https://github.com/prasunanand/gsoc_blog/blob/master/img/sciruby_blog/arrayfire/sub.png?raw=true "Fig. 2: Matrix Subtraction")
![Matrix Multiplication](https://github.com/prasunanand/gsoc_blog/blob/master/img/sciruby_blog/arrayfire/mult.png?raw=true "Fig. 3: Matrix Multiplication")
![Matrix Determinant](https://github.com/prasunanand/gsoc_blog/blob/master/img/sciruby_blog/arrayfire/det.png?raw=true "Fig. 4: Matrix Determinant")
![LU Decomposition](https://github.com/prasunanand/gsoc_blog/blob/master/img/sciruby_blog/arrayfire/lu.png?raw=true "Fig. 5: LU Decomposition")

(Note: The above benchmarks have been done on an AMD FX 8350 octacore processor and Nvidia GTX 750Ti GPU. CUDA backend of ArrayFire was used with double floating points.)

The figure shows that ArrayFire takes the least computation time of all. For elementwise arithmetic operations, ArrayFire is 2 e 4 times faster than NMatrix for Ruby whereas 2 e 3 times faster than NMatrix for JRuby.

The figure shows that ArrayFire takes the least computation time of all. ArrayFire is 3 e +6 times faster than NMatrix for JRuby and NMatrix for Ruby(not BLAS) whereas 7 e +5 times faster than NMatrix for Ruby(using BLAS).

For LAPACK routines, like calculating determinant and lower-upper factorization, ArrayFire is 100 times faster than NMatrix for JRuby whereas 6 times faster than NMatrix for Ruby(using LAPACKE).

# Application

The GSoC 2017 application can be found [here](https://github.com/prasunanand/resume/wiki/GSoC-2017-proposal).

# Code

[ArrayFire-rb](https://github.com/prasunanand/arrayfire-rb): The [pull request](https://github.com/arrayfire/arrayfire-rb/pull/3) is undergoing a review.

ArrayFire-rb Benchmarks: Codebase can be found [here](https://github.com/prasunanand/arrayfire-rb-benchmark-suite).

Bio::FasterLmmD : Codebase can be found [here](https://github.com/prasunanand/bio-faster_lmm_d)


The work on creating the bindings have been explained in the following nine blog posts:

1. [Ruby C extensions for complex projects](http://www.prasunanand.com/ruby-c-extensions/2017/06/23/gsoc17-ruby-c-extensions-for-complex-projects.html)
2. [Installation](http://www.prasunanand.com/arrayfire/2017/06/23/gsoc17-arrayfire-ruby-bindings-part-1-installation.html)
3. [Af_Array](http://www.prasunanand.com/arrayfire/2017/07/04/gsoc17-arrayfire-ruby-bindings-part-2-af_array.html)(see performance)
4. [Test-suite and Algorithm class](http://www.prasunanand.com/arrayfire/2017/07/22/gsoc17-arrayfire-ruby-bindings-part-3-minitest-algorithm.html)
5. [BLAS and LAPACK routines](http://www.prasunanand.com/arrayfire/2017/08/16/gsoc17-arrayfire-ruby-bindings-part-4-blas-lapack.html)(see performance)
6. [Statistics and Random Engine routines](http://www.prasunanand.com/arrayfire/2017/08/17/gsoc17-arrayfire-ruby-bindings-part-4-statistics-and-random-engine.html)
7. [Device and Util](http://www.prasunanand.com/arrayfire/2017/08/22/gsoc17-arrayfire-ruby-bindings-part-6-device.html)
8. [Multiple Backends: CUDA, OpenCL and CPU](http://www.prasunanand.com/arrayfire/2017/08/24/gsoc17-arrayfire-ruby-bindings-part-7-backend-cuda-and-opencl.html)
9. [ArrayFire-NMatrix Interface](http://www.prasunanand.com/arrayfire/2017/08/24/gsoc17-arrayfire-ruby-bindings-part-8-nmatrix-interface.html)


I took a side-track working on `Bio::FasterLmmD` . This work is not complete and still in progress.
It is an effort to `call D from Ruby`. The work has been explained in a previous [blog post](/gpu-computing/2017/07/25/gsoc17-calling-d-from-ruby-for-gpu-computing.html).

The work on ArrayFire-rb - JRuby has been postponed for now as I wanted to concentrate on MRI for
the best results.

# Future Work

The future work involves improving the ArrayFire-rb code and writing tutorials. ArrayFire is not limited to
linear algebra so I will create bindings for Signal Processing, Computer Vision, etc. I will also add support
for data types other than `double`.

The work on ArrayFire-rb - JRuby will begin as soon as ArrayFire gem is published.

# FOSS

This has been my second GSoC with SciRuby. It has been more than an year contibuting extensively to FOSS.

I really appreciate the effort by Google Open Source Committee for conducting GSoC every year. It is the
best platform for the aspiring programmers improve their skill and give back to society by developing free
and open source software.

Last year's GSoC work helped me to present a talk at FOSDEM 2017 and Ruby Conf India 2017.  I got active
in the Indian Ruby Community. Recently, I have been invited as a speaker to Ruby World Conference 2017, Matsue, Japan
and RubyConf 2017, New Orleans, to talk on "GPU computing with Ruby".

I plan to continue contributing to open source, strive for improving my skills, and help new programmers
contribute to FOSS. I would be glad if I could mentor students for upcoming GSoCs.

# Acknowledgements

I would like to express my sincere gratitude to my mentor Pjotr Prins, for his guidance, patience and support.
I have learn a lot from him since my last GSoC and still learning. I couldn't have hoped for a better mentor.

I am grateful to Google and the Ruby Science Foundation for this golden opportunity.

I am very thankful to John Woods, Sameer Deshmukh, Alexej Gossmann, Gaurav Tamba and Pradeep Garigipati
who mentored me through the project.
