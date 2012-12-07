---
layout: post
title: "First NMatrix Alpha Released"
date: 2012-04-11 13:36
comments: true
author: John Woods
categories: [Algorithms,Matrices,NMatrix]
---
Two months ago, I [mentioned the existence of a prototype Ruby linear algebra library](2012/02/11/numeric-matrix-considerations), written in C.

I am pleased to announce that yesterday we released our first alpha of said library, [NMatrix](/nmatrix/) v0.0.1.<!--more-->

## Creating Matrices

There are lots of different ways to create matrices. The first and easiest is to supply dimensions and initial data:
<pre><code>>> n = NMatrix.new(4, 0) # a square 4x4 dense zero matrix
=> #&lt;NMatrix:0x9a57e14shape:[4,4] dtype:int32 stype:dense&gt;
>> n.pretty_print
0  0  0  0
0  0  0  0
0  0  0  0
0  0  0  0
=> nil
</code></pre>

## Data Types

You may notice that this first matrix defaulted to <tt>dtype=:int32</tt>. NMatrix will try to guess the <i>dtype</i> based on the
first initial value you provide (e.g., <tt>NMatrix.new(4, [0.0, 1])</tt> will be :float32), but you can choose to provide
a <i>dtype</i> in addition to or in lieu of initial values:

<pre><code>>> n = NMatrix.new(4, [0,1], :rational128)
=> #&lt;NMatrix:0x9959e04shape:[4,4] dtype:rational128 stype:dense&gt;
>> n.pretty_print
0/1  1/1  0/1  1/1
0/1  1/1  0/1  1/1
0/1  1/1  0/1  1/1
0/1  1/1  0/1  1/1
>> m = NMatrix.new(4, :int64)  # no initialization of values
=> #&lt;NMatrix:0x99fad68shape:[4,4] dtype:int64 stype:dense&gt;
>> m.pretty_print
-1217641248  161386160  161386100  161385680
161385640  161385520  161385420  161384180
161384120  161381060  161381020  161412140
161411940  161411880  161411840  160879240
=> nil
</code></pre>

## Storage Formats

The storage type (<i>stype</i>) can also be specified, prior to the dimension argument. However, with sparse storage formats, initial values don't make sense, and these matrices will contain zeros by default.

<pre><code># empty list-of-lists-of-lists 4x3x4 matrix
n = NMatrix.new(:list, [4,3,4], :int64)

# Ruby objects in a 'Yale' sparse matrix
m = NMatrix.new(:yale, [5,4], :object)

# A byte matrix containing a gradient
o = NMatrix.new(:dense, 5, [0,1,2,3,4], :byte)
</code></pre>

The matrix <tt>m</tt> created above is a Yale-format sparse matrix, or more specifically, "new Yale," which differs from "old Yale" in that the diagonal is stored separately from the non-diagonal elements. Thus, diagonals can be accessed and set in constant time.

Currently, all storage is row-based.

### Conversion

You can also convert between any of these three stypes using <tt>cast</tt>, e.g.,

<pre><code>n = NMatrix.new(:list, 4, :int64)
n[0,0] = 5
n[0,3] = -2
dense = n.cast(:dense, :int64)
</code></pre>

## Vectors

Currently, only dense vectors are implemented as a child class of <tt>NMatrix</tt>, and creation is similar:

<pre><code>>> nv = NVector.new(5, :int64)
=> #&lt;NVector:0x9a62328shape:[5,1] dtype:int64 stype:dense orientation:column&gt;
</code></pre>

## Math Operations

Most element-wise mathematical operations are supported for Yale and dense types. These use the basic operators (e.g., +, -, /, *, ==).

For non-element-wise matrix multiplication, use the <tt>dot</tt> instance method of NMatrix. Whole-matrix comparison (returning a single boolean value) is the <tt>equal?</tt> or <tt>eql?</tt> method.

## Road Map

Much more remains to be written than has been completed. Here are some of our key priorities:

 * determinants
 * matrix-vector multiplication for Yale
 * adaptation of SciRuby Matlab file reader to support NMatrix
 * in-place transposition

If you want to get involved, I suggest visiting the [NMatrix issue tracker](https://github.com/sciruby/nmatrix/issues). It will contain not only bugs, but also features that need to be implemented.

## Conclusion

We're all pretty excited about NMatrix. But we couldn't have gotten this far without Masahiro Tanaka's [NArray](http://narray.rubyforge.org/), which has served as a model for our library.

And we can't do it without your help. A numerical library in Ruby is no small endeavour, and probably requires at least one full-time programmer (which we do not have). Please consider contributing, even if it's just by letting us know what you think.

Happy coding!
