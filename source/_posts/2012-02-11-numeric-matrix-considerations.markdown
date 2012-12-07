---
layout: post
title: "Numeric Matrix Considerations"
date: 2012-02-11 17:35
comments: true
author: John Woods
categories: [Algorithms,Matrices,NMatrix]
---
Maybe you've been wondering why the SciRuby people have been so quiet lately.

Mostly, we've been working on [NMatrix](http://github.com/SciRuby/nmatrix), which is our prototype linear algebra library, written as a Ruby C extension.<!--more-->

So far, we have dense and linked-list-of-linked-lists (or just "list") matrices, which can exist in an essentially unlimited number of dimensions. This week, I implemented the [beginnings of a two-dimensional sparse type known as "new Yale."](https://github.com/SciRuby/nmatrix/blob/sparse/ext/nmatrix/yale.c) If you're curious, check out _[Numerical Recipes in C](http://apps.nrbook.com/c/index.html)_, pp. 78-79.

"New Yale" differs from "old Yale" (which SciPy uses for its CSR type) in that the diagonals are accessible in <tt>O(1)</tt> instead of <tt>O(log(n))</tt> time. Most libraries for Yale matrices include conditionals that permit both types to be used together (e.g., multiplying a new Yale matrix by an old Yale matrix).

As I worked through implementation of matrix multiplication for Yale, I realized that matrix libraries are a bottomless pit. One could probably write matrix algorithms forever and still have lots left to do.

So we need to set ourselves some limits.

It's not just whether the diagonal is extracted from the rest of the matrix. It's also whether you want to be able to multiply integer-valued matrices by real-valued matrices without an expensive copy operation. It's whether you want to be able to multiply dense against Yale matrices or two Yale and list matrices that have different dtypes, or have separate storage for symmetric matrices, and so on.

Some of this can be templated using generators. But the complexity begins to grow to the point where it's infeasible to test everything you've implemented. (You also start to get lost in a magical world of function pointers, which is yet another concern.)

So, again, we need to set ourselves some limits.

Where those limits might be, however, I know not. Weigh in on the Google Group, in the comments, or on [our Google+ page](https://plus.google.com/109304769076178160953/posts)!
