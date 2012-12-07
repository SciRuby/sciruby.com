---
layout: post
title: "How to use NMatrix's shortcuts"
date: 2012-12-06
comments: true
author: Carlos Agarie
categories: [NMatrix]
---

John Woods [merged my last pull request to NMatrix](https://github.com/SciRuby/nmatrix/commit/2b480ce0985affc7218fc341fcb4e5024b30545b) recently and I wanted to write about why we created these shortcuts and what can be done with them.

## The need of shortcuts

Originally, the NMatrix shortcuts were written by [Daniel Carrera](https://github.com/dcarrera) and sent to the mailing list, where I started working with them. Some discussions, decisions and 2 pull requests later, we have a working set of methods to create common matrices (`zeros`, `ones`, `random`, etc) and do some cool stuff (`column` and `linspace`) very easily.

I think that there are at least 2 good reasons to have them: MATLAB users are going to feel at home and they allow us to experiment very rapidly. For example, it's dead simple to create an identify matrix when you need one:

	require 'nmatrix'

	matrix = NMatrix.identity(3) # 3x3
	=> #<NMatrix:0x007faa5b8cda40shape:[3,3] dtype:float64 stype:dense> 
	matrix.pp

	# [1.0, 0.0, 0.0]
	# [0.0, 1.0, 0.0]
	# [0.0, 0.0, 1.0]

And a random matrix to test some operation:

	rand_matrix = NMatrix.random(4) # 4x4
	=> #<NMatrix:0x007faa5b01cdd8shape:[4,4] dtype:float64 stype:dense>
	rand_matrix.pp

	# [0.5933103148378577, 0.8556103970281977, 0.7176768395610358, 0.07160353964395305]
	# [0.8566365570076784, 0.33925854948960343, 0.5994298479703805, 0.6906794137948586]
	# [0.5684593743073485, 0.10273166792035393, 0.46187577773882293, 0.586085963973687]
	# [0.8944982538369494, 0.9896291945923837, 0.8787034784673503, 0.5933103148378577]

## Shortcuts available

The following list is taken from [NMatrix's wiki](https://github.com/SciRuby/nmatrix/wiki/NMatrix).

	ones(3,3)                    # A 3x3 matrix of ones.
	zeros(4,4) or zeroes(4,4)    # Creates a matrix of zeros with dimensions as parameters.
	identity(5,5) or eye(5,5)    # A 5x5 identity matrix. Only works with NMatrix.
	seq(8)                       # An 8-vector with a sequence of values from 0 to 7.
	random(6,6)                  # A 6x6 matrix of random values between 0 and 1.
	
	column(2)                    # Return the 2nd column of the original matrix. Only works with NMatrix.
	
	indgen(8)                    # To make IDL users happy. Same as `seq(8, :int32)`.
	findgen(8)                   # Same as `seq(8, :float32)`.
	bindgen(8)                   # Same as `seq(8, :byte)`.
	cindgen(8)                   # Same as `seq(8, :complex64)`.

	linspace(0, pi, 100)         # A vector of 100 values from 0 to pi, inclusive. Only works with NVector.

The method names are mostly from MATLAB. In the future, when more functionality is added, we'll try to create a more "Ruby-like" feel to the API. But as these shortcuts do very primitive tasks, there isn't much room to improvement. (prove me wrong, please!)

I'll create a separate wiki page for the shortcuts to be able to organize and put more information about how to use them. But I want to create a better RDoc documentation for NMatrix - so the full API will probably be referenced there. I'll post a new article here when something along these lines is done.

## Examples

First, let's try normalizing the columns of a NMatrix.

	require 'nmatrix'

	m = NMatrix.seq(3, :float32)
	m.pp

	# [0.0, 1.0, 2.0]
	# [3.0, 4.0, 5.0]
	# [6.0, 7.0, 8.0]

	(0 .. m.shape[1] - 1).each do |j|
	  col = m.column(j)
		norm = Math.sqrt(col.transpose.dot(col)[0, 0])
		(0 .. m.shape[0] - 1).each { |i| m[i,j] /= norm }
	end

	m.pp

	# [0.0, 0.12309148907661438, 0.20739033818244934]
	# [0.4472135901451111, 0.4923659563064575, 0.5184758305549622]
	# [0.8944271802902222, 0.861640453338623, 0.8295613527297974]

	(0 .. m.shape[1] - 1).each do |j|
	  col = m.column(j)
	  puts Math.sqrt(col.transpose.dot(col)[0,0])
	end

	# 0.9999999701976772
	# 1.0
	# 1.0

Unfortunately, slice by reference isn't working on HEAD, so we can't make this code a bit smarter (see issue [#51](https://github.com/SciRuby/nmatrix/issues/51)).

The other thing that I've wanted to do is to use `linspace` to generate points for plotting sines, cosines, etc. Let me show an example:

	require 'nmatrix'

	vector = NVector.linspace(0, Math::PI, 10)
	vector.pretty_print

	# 0.0
	# 0.3490658503988659
	# 0.6981317007977318
	# 1.0471975511965976
	# 1.3962634015954636
	# 1.7453292519943295
	# 2.0943951023931953
	# 2.443460952792061
	# 2.792526803190927
	# 3.141592653589793
	
	10.times do |i|
	  vector[k, 0] = Math::sin(vector[k, 0])
	end
	
	vector.pretty_print
	
	# 0.0
	# 0.3420201433256687
	# 0.6427876096865393
	# 0.8660254037844386
	# 0.984807753012208
	# 0.984807753012208
	# 0.8660254037844388
	# 0.6427876096865395
	# 0.3420201433256689
	# 1.2246467991473532e-16
	
These results show something good: we are able to use NMatrix for simple tasks already. Of course, there are problems to be solved, e.g. slice by reference, inversion for all dtypes, eigenvalues. But it's going somewhere, it's growing. 

I hope that by this time next year we'll have a very mature linear algebra library (and much more).