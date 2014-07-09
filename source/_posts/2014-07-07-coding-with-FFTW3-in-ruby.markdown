---
layout: post
title: "FFTW3 API Design"
date: 2014-23-06 21:04
author: Magdalen Berns
comments: true
categories: FFTW, GSoC 2014, thisMagpie,GSoC
---
Coding an FFTW3 Wrapper in Ruby

------

[ ![Fork the FFTW Github repository][1]](https://github.com/thismagpie/fftw)

![FFTW development Logo](http://fftw.org/fftw-logo-med.gif)![Ruby](https://www.pivotaltracker.com/wp-content/uploads/2013/12/ruby-logo.png)

-------
It is not possible to explain what I am doing this project in depth and be understood, without covering some details about `FFTW` itself, to outline why the module, works so well at performing FFT operations so **quickly**.

The underlying problem analogue to digital conversion presents us with, as technology users, is that there is an inevitable trade off between a result that is computationally expensive (i.e. slow) to reproduce but more precise and less degraded and a result that is less computationally expensive to reproduce but is also less precise and more degraded.

Fast Fourier Transform (FFT) is a technique which works around this problem by [decomposing the Discrete Fourier Transform (DFT) matrix recursively](http://www.fftw.org/fftw-paper-ieee.pdf).


`FFTW3 API` ruby wrapper allows NMatrix users to require the gem and query the `FFTW3` interface in order to perform `FFTW` operations on `nmatrix d-type` objects.

## FFTW3 Design and Implementation

[There is a great paper on how the FFTW API has been designed](http://www.fftw.org/fftw-paper-ieee.pdf) (which is pretty vast!) but for the lazy, I will summarise some points of notes about the FFT3 API.

### Designing the FFTW wrapper gem

It was determined that dynamic array allocation was appropriate for working with nmatrix and fftw. This decision is supported by the [recommendation to allocate most arrays dynamically](http://www.fftw.org/doc/Dynamic-Arrays-in-C.html#Dynamic-Arrays-in-C) with with fftw_malloc which is made by the FFTW3 developers themselves and my mentor and  Colin Fuller has agreed this was the best course of action, too.

```
    fftw_complex *an_array;
    an_array = (fftw_complex*) fftw_malloc(rank * sizeof(fftw_complex));
```

That said, dynamic arrays is slightly more awkward doing this in in some ways in terms of accessing the array elements.
[According to the  FFTW3 API Documentation this accessing dynamic array elements is best done using c++ ](http://www.fftw.org/doc/Dynamic-Arrays-in-C.html#Dynamic-Arrays-in-C) by creating class
 e.g. `cFFTW` and then overloading it with the `()` operator.

----------

### FFTW3 Codelets

----------

There is not a single procedure which computes the FFT in FFTW3 but rather, the `FFTW3 API` comes complete with a set of so called ```codelets``` which are executed at runtime by a `planner` which chooses the appropriate codelets to run based on what the genfft compiler learns about the hardware capability of the machine which runs the code. Each codelet is capable of [performing a FFT on data of a small size](https://www.cs.drexel.edu/~jjohnson/2009-10/winter/cs650/lectures/pldi99-slides.pdf).

-----------

### FFTW3 Planners

-----------
There is no need to create a `wrapper` function in `c++` for `planner` functions because they are designed to be executed at runtime so the process works more like this:

1. create a ruby specific c++ wrapper fftw module function which takes an `nmatrix` object holding an `nmatrix d-type data array` which stores the DFT to be processed.
2. The `nmatrix d-type data array` data which has a size that the compiler (`genfft`) can read to determine which `codelets` to use.
3. the `plan` is called at `runtime` when its wrapper function is initialised
4. When the plan is called the appropriate codelets are executed at `runtime`.

-------

#### Plans of Interest

Since **multidimensional** arrays are more flexible to work with than 1D or 2D arrays so the plans of interest for my project are the **multidimensional** ones.

The following is sourced from the FFTW3 API [documentation page on planner flags](http://www.fftw.org/doc/Planner-Flags.html)

A real-to-real, `r2r` **multidimentional plan**



A real-to-complex, `r2c` **multidimentional plan**:

```
fftw_plan fftw_plan_dft_r2c(int rank, const int *n,
                                 double *in, fftw_complex *out,
                                 unsigned flags);
```

----------


A **multidimentional plan**: on data with arbitrary rank:

     fftw_plan fftw_plan_dft(int rank, const int *n,
                             fftw_complex *in, fftw_complex *out,
                             int sign, unsigned flags);
                             

#### Planning-rigor flags

These `flags` store constant values to be taken as an argument by a `FFTW3 plan`

-------

`FFTW_ESTIMATE`

> Specifies that, instead of actual measurements of different algorithms, a simple heuristic is used to pick a (probably sub-optimal) plan quickly. With this flag, the input/output arrays are not overwritten during planning.

-------

`FFTW_MEASURE`

> Tells `FFTW` to find an optimized plan by actually computing several FFTs and measuring their execution time. Depending on your machine, this can take some time (often a few seconds). `FFTW_MEASURE` is the default planning option.

-------

`FFTW_PATIENT`

>  Like `FFTW_MEASURE`, but considers a wider range of algorithms and often produces a “more optimal” plan (especially for large transforms), but at the expense of several times longer planning time (especially for large transforms).

-------

`FFTW_EXHAUSTIVE`

> Like `FFTW_PATIENT`, but considers an even wider range of algorithms, including many that we think are unlikely to be fast, to produce the most optimal plan but with a substantially increased planning time.

-------

`FFTW_WISDOM_ONLY`

> A special planning mode in which the plan is only created if wisdom is available for the given problem, and otherwise a NULL plan is returned. This can be combined with other flags, e.g. `FFTW_WISDOM_ONLY | FFTW_PATIENT` creates a plan only if wisdom is available that was created in `FFTW_PATIENT` or `FFTW_EXHAUSTIVE` mode. `The FFTW_WISDOM_ONLY` flag is intended for users who need to detect whether wisdom is available; for example, if wisdom is not available one may wish to allocate new arrays for planning so that user data is not overwritten.

-------

#### Algorithm-restriction flags

---------


`FFTW_DESTROY_INPUT`
> specifies that an out-of-place transform is allowed to overwrite its input array with arbitrary data; this can sometimes allow more efficient algorithms to be employed.

-------

`FFTW_PRESERVE_INPUT`

> specifies that an out-of-place transform must not change its input
> array. This is ordinarily the default, except for `c2r` and `hc2r` (i.e.
> complex-to-real) transforms for which `FFTW_DESTROY_INPUT` is the
> default. In the latter cases, passing `FFTW_PRESERVE_INPUT` will attempt
> to use algorithms that do not destroy the input, at the expense of
> worse performance; for multi-dimensional `c2r` transforms, however, no
> input-preserving algorithms are implemented and the planner will
> `return NULL` if one is requested.

-------

`FFTW_UNALIGNED`

> Specifies that the algorithm may not impose any unusual alignment
> requirements on the input/output arrays (i.e. no SIMD may be used).
> This flag is normally not necessary, since the planner automatically
> detects misaligned arrays. The only use for this flag is if you want
> to use the new-array execute interface to execute a given plan on a
> different array that may not be aligned like the original. (Using
> fftw_malloc makes this flag unnecessary even then. You can also use
> fftw_alignment_of to detect whether two arrays are equivalently
> aligned.)

-------

#### Limiting Planning Time Function

    extern void fftw_set_timelimit(double seconds);

> This function instructs `FFTW` to spend at most seconds seconds
> (approximately) in the planner. `If seconds == FFTW_NO_TIMELIMIT` (the
> default value, which is negative), then planning time is unbounded.
> Otherwise, `FFTW` plans with a progressively wider range of algorithms
> until the the given time limit is reached or the given range of
> algorithms is explored, returning the best available plan.
> 
> For example, specifying `FFTW_PATIENT` first plans in `FFTW_ESTIMATE`
> mode, then in `FFTW_MEASURE` mode, then finally (time permitting) in
> `FFTW_PATIENT`. If `FFTW_EXHAUSTIVE` is specified instead, the planner
> will further progress to `FFTW_EXHAUSTIVE` mode.
> 
> Note that the seconds argument specifies only a rough limit; in
> practice, the planner may use somewhat more time if the time limit is
> reached when the planner is in the middle of an operation that cannot
> be interrupted. At the very least, the planner will complete planning
> in `FFTW_ESTIMATE` mode (which is thus equivalent to a time limit of
> 0).

-------


There is a lot more to say about this API but I must try to break up the information to keep things more interesting and give myself time to code the project! 

Just to comment on the point that, this project has been exceptionally rewarding and challenging in various ways, so far and I feel really quite lucky to be able to work on it full time all summer.

###FFTW Gem Feedback

I am hopeful [SciRuby](http://sciruby.com) readers are now placed in a better position to be able to relate to the discussion in my updates on the FFTW3 Sciruby GSoC project this year now
[github repository bug tracker](https://github.com/thismagpie/fftw/issues))

That said, please feel free to ask questions about [the project](https://github.com/thismagpie/fftw)) (in the comments section or on the [github repository bug tracker](https://github.com/thismagpie/fftw/issues)), try out the code and if you find any bugs, have any feedback, suggestions or questions for me please let me know!

-------

#### **Help Send me 10 year Reunion in San Jose this October**
I have won the lottery invite for the google 10 year reunion in San Jose to meet the delegates from SciRuby and the rest of FOSS community (who have been involved in GSoC over the past decade) but need to travel all the way from Scotland and accordingly am paying off an overdraft so I am asking the community to help towards the costs of the travel, so I can attend:-)


All donations large and small, are gratefully received. The generosity of the community so far has been overwhelming so far, so thanks again to everyone who has chipped in so far!

---------

[![Donate with Pledgie][4]](https://pledgie.com/campaigns/25892)  [![Donate with Pledgie][5]](https://pledgie.com/campaigns/25907)
==========

**Bitcoin: 1129CMrZkqKt32F5kJhYiNMsfhRnWFEeTA**

-------

####Thanks For Reading 

---------------


  [1]: https://s3.amazonaws.com/github/ribbons/forkme_left_darkblue_121621.png
  [4]: https://pledgie.com/campaigns/25892.png?skin_name=chrome
  [5]: https://pledgie.com/campaigns/25907.png
