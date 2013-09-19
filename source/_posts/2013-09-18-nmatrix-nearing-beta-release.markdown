---
layout: post
title: "NMatrix nearing beta release"
date: 2013-09-18 15:18
comments: true
author: John Woods
categories: [NMatrix]
---
As of this writing, NMatrix v0.0.9 is available on RubyGems. It is likely that the next version will be a beta release
candidate, as there's only one critical feature still missing (`==` between matrices of different storage types).

An enormous amount has changed since my last entry.

New, friendlier constructor
---------------------------

First and foremost, NMatrix sports a new constructor, based
on [helpful comments from folks on the listserv](https://groups.google.com/d/msg/sciruby-dev/tPwnmPbRR_U/PL0Nc92gdzEJ). Here
are some examples of construction:

    NMatrix.new([4,4], 0) # 4x4 dense matrix of :int32, all 0
    NMatrix.new([4,4], 0.0) # 4x4 dense matrix of :float64, all 0.0
    NMatrix.new([4,4], 0.0, dtype: :complex64) # 4x4 dense matrix of :complex64
    NMatrix.new([1,4], stype: :yale) # size 4 sparse (Yale) row vector of 0s
    NMatrix.new([4,3], [0,1,2]) # 4 rows, 3 columns: gradient across each row from 0 to 2 (int32)
    NMatrix.new([4,3], [0,1,2], stype: :yale, default: 0) # same as above, but Yale storage (int32)
    NMatrix.new([4,1], stype: :list, dtype: :rational128) # size 4 sparse (list) column vector of rational 0s
    NMatrix.new([4,4]) # 4x4 dense matrix containing nils
    NMatrix.new([4,4], dtype: :int64) # 4x4 uninitialized dense matrix containing 64-bit integers
    NMatrix.new(4)     # same as above

The different storage types (stypes) have slightly different behaviors when no initialization value is provided. This
may change in the future, but addresses the somewhat different use-cases of these storage types.

I show a variety of examples above, several of which are not particularly wise uses --- for example, the `[0,1,2]` Yale
gradient, which uses 32-bit integers and must store 11 column indices and pointers (which are most likely unsigned
long integers) in addition to the 11 entries (4 for the always-stored diagonal, 1 for the default, and 6 for the
non-diagonal non-zeros).

The key to understanding the differing construction is the default value. All sparse matrices need a default. Usually we
think of sparse matrices as being zero-based (only non-zero values are stored), but NMatrix now allows other defaults.
However, dense matrices don't need defaults, and in fact sometimes it's more efficient to allocate them without
initializing them --- such as if they're about to store the results of some LAPACK function call. But if they contain
Ruby objects, they **have** to be initialized, which is why they become nil.

We're still working out a few bugs in construction. If you find any, please report them in our [issue tracker](http://github.com/SciRuby/nmatrix/issues).

Elimination of NVectors
-----------------------

We removed the NVector class. Frankly, it didn't make sense. A vector can't have an orientation unless it has multiple
dimensions --- even if some of those dimensions have length 1. Now, vectors and matrices are treated the same in the code.

The good news is that element access (`[]`) has been rewritten so that the programmer only needs provide the coordinates
whose dimensions are not 1. For example:

    n = NMatrix.new([4,1,3], 0) # 3-dimensional matrix of 0s
    n[2,1] == n[2,0,1]
    => true

The same rule applies to slicing.

Slicing improvements
--------------------

Aleksey Timin contributed the framework for matrix slicing. Matrices can be sliced by copying or by reference. Modifying
a copy-slice does not modify the original matrix; but modifying a reference slice does.

A copy of some portion of the matrix may be made using `NMatrix#slice`:

    new_matrix = n.slice(0..1,0..2)

And what appears to be a copy, but is actually a reference, may be made using brackets:

    ref = n[0..1,0..2]

Reference slices may also be modified in a variety of ways:

    n[0..4,2..8] = 0   # change all entries to 0
    n[0..4,2..8] = [0,1,2]  # change the selected entries to [0,1,2,0,1,2,0,1,2...]
    n[0..4,2..8] = NMatrix.new([3,3], [0,1,2]) # same as above

For the sake of speed, each of these `[]=` returns the right-hand value, whether that value is an array, matrix, or
single value. So, you can do this:

    x = m[0..4,2..8] = n[0..1,0..2] = [1,2,3]

and `x` will be equal to `[1,2,3]` after the evaluation.

Iteration
---------

Matrices are now enumerable. There are a variety of enumerators --- namely `each_with_indices` (and `each`), `each_stored_with_indices`,
and `each_ordered_stored_with_indices`. The first, `each`, is guaranteed to produce the same iteration regardless of the
storage type. The other two iterate only across the stored entries.

Elementwise comparisons
-----------------------

A regular matrix comparison, returning a single boolean, can be accomplished using `==` or `!=`. In earlier versions of
NMatrix, the results of the element-wise versions, `=~`, `!~`, `>`, `<`, `>=`, and `<=`, were matrices of 0s and 1s,
stored using the `:byte` dtype.

Now, these comparisons return matrices of Ruby objects:

    n < m
    => [ true,  false, false, true,
         false, false, false, true,
         true,  true,  true,  true  ]

Try experimenting with sparse matrices to see how the default value (`#default_value`) is initialized on the result during an
elementwise comparison.

Chunky bacon
------------

I'm really excited about all of the chunky bacon in our latest release. I feel like things are really coming together
for our library. I'm also glad to see that people are using it.

If you're thinking of using NMatrix yourself, I strongly encourage it. Although I'm writing my dissertation, I plan to
prioritize troubleshooting ahead of just about everything else. I want using NMatrix to be an easy choice for every
Rubyist.

Thanks for reading!





