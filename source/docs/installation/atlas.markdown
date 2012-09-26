---
layout: page
title: "ATLAS installation instructions"
date: 2012-09-26 15:28
comments: false
sharing: true
footer: true
author: Eric MacAdie, John Woods
---

[More detailed instructions](http://math-atlas.sourceforge.net/atlas_install/) are available from ATLAS' website. Please
consult them first if you have trouble.

Turn off CPU throttling
-----------------------

The first three letters in ATLAS stand for "automatically tuned." That means that during ATLAS configuration and installation,
your system is tested in various ways to determine the optimal algorithms to use, e.g., for matrix multiplication. That means
you need to turn off CPU throttling.

<h3>Mac</h3>

If you're using a Macbook, you'll really want to use the version of ATLAS provided by Apple. We aren't aware of any easy
way to disable throttling. So, if you haven't done so already, go install [Apple Developer Tools](https://developer.apple.com/technologies/tools/).

<h3>Ubuntu</h3>

<pre><code>sudo apt-get install indicator-cpufreq
</code></pre>

Installing this package will add an applet to your menu bar. You should select "performance" mode.

LAPACK and ATLAS
----------------

You don't actually have to untar or compile LAPACK. You just need the tar file in order to compile ATLAS. You can obtain
it from [Netlib](http://www.netlib.org/lapack).

Download the most recent *stable* version of [ATLAS](http://math-atlas.sourceforge.net/) and <tt>bunzip2</tt> it.

You can't actually run configure in the untarred directory. Instead, do something like this:

<pre><code>/home/$USER/ATLAS/configure -Fa alg -fPIC \
  --prefix=/home/$USER/outputatlas \
  --incdir=/home/$USER/outputatlas/include \
  --libdir=/home/$USER/outputatlas/lib \
  --with-netlib-lapack-tarfile=/home/$USER/lapack-3.4.1.tar

  cd /home/$USER/outputatlas
  make
  make install
</code></pre>

That should take at least twenty minutes, so go grab a healthy snack. Or some chunky bacon.

<h3>Manual installation</h3>

It's possible that make install may fail. If so, Eric recommends manually installing the <tt>.a</tt> and header files like so:

<pre><code>mkdir /usr/local/atlas
cp -v /home/$USER/outputatlas/lib/*.a /usr/local/atlas/
ln -s /usr/local/atlas/libcblas.a /usr/lib/libcblas.so
ln -s /usr/local/atlas/libatlas.a /usr/lib/libatlas.so
ln -s /usr/local/atlas/liblapack.a /usr/lib/liblapack.so

mkdir /usr/local/atlas/include/
cp /home/$USER/ATLAS/include/cblas.h /usr/local/atlas/include/
</code></pre>

<h3>Ubuntu</h3>

On Ubuntu, you may be able to get away with just installing the ATLAS packages:

<pre><code>sudo apt-get install libatlas-dev libatlas3gf-base
sudo ln -s /usr/lib/libgslcblas.so.0.0.0 /usr/lib/libcblas.so
sudo ln -s /usr/lib/atlas-base/libatlas.so.3gf.0 /usr/lib/libatlas.so
gem install nmatrix
</code></pre>