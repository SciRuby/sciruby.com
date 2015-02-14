---
layout: post
title: "NMatrix's first beta release (belated announcement)"
date: 2015-02-14 14:56
comments: true
author: John Woods
categories: [Algorithms,Matrices,NMatrix]
---
Almost two months ago, in December, we [release our first beta of NMatrix](https://rubygems.org/gems/nmatrix), our
linear algebra library for the Ruby language.

Rather than discussing what NMatrix has (which you can find in the [readme](https://github.com/SciRuby/nmatrix/blob/master/README.rdoc)),
I want to take this opportunity to discuss where we'd like it to go in the future. Think of this as our roadmap to 1.0.

External Library Requirements
-----------------------------

NMatrix currently has an extremely limited set of Ruby gem dependencies &mdash; essentially only packable, which is
needed for some of the I/O functionality &mdash; but as with other linear algebra libraries, its native library
requirements can be problematic.

Specifically, NMatrix requires ATLAS &mdash; which is virtually impossible to install on many Macs, and is no longer
readily available with Yosemite &mdash; and also likes to have LAPACK around. Moreover, some would prefer to use other
math libraries with NMatrix.

There are several partial solutions. The clearest is that any components of NMatrix which require external non-standard
libraries need to be abstracted into separate gems &mdash; such as <tt>nmatrix-atlas</tt> and <tt>nmatrix-io</tt> &mdash; and
that these components should be interchangeable with others. NMatrix then needs to be able to fall back on either native
C/C++ or Ruby implementations of certain functionalities.

To this end, a number of simple LAPACK and CBLAS functions have been implemented in NMatrix, with volunteers working on
a few others (the list is available in the README). [One of our proposed Google Summer of Code 2015 project ideas](https://github.com/SciRuby/sciruby/wiki/Google-Summer-of-Code-2015-Ideas#abstraction-of-atlascblasclapack-or-openblas-into-a-separate-gem) involves
breaking up NMatrix into several gems.

The end result will be a much simpler installation process, and more freedom of choice.

Removal of Unnecessary Features
-------------------------------

We thought it would be quite clever to include rational number support in NMatrix, including three types (32-bit, 64-bit,
and 128-bit). Unfortunately, these types increase compilation time by about 30% (back-of-the-envelope calculation), and
aren't particularly useful since most matrix operations with rationals as inputs give irrational-valued matrices
as their outputs. The inclusion of rational types also complicates the codebase.

Additionally, rationals are computationally problematic; what happens if there is overflow? Ruby handles overflow gracefully
in its own rational type, which NMatrix users would still be able to utilize via the <tt>:object</tt> dtype.

You
---

The biggest change going forward is that NMatrix development needs to support actual use-cases. In the past, we've aimed
to flesh out support for sparse storage formats, as well as math operations for the dense storage-type. It is folly to
develop features no one wants to use. Future core development, then, will aim to make NMatrix's current feature set more
usable.

That means that future non-core development depends on you. What would you like to see in NMatrix? What would make it
more useful for you?

If you want to become involved, now is a good time. We're getting ready to submit a Google Summer of Code application
(our third, I believe), and we're in need of students &mdash; <em>and especially mentors</em>. Please consider joining
our [mailing list](https://groups.google.com/forum/#!forum/sciruby-dev) and adding yourself as a mentor on the
[ideas page](https://github.com/SciRuby/sciruby/wiki/Google-Summer-of-Code-2015-Ideas).

Thanks so much for your support!

