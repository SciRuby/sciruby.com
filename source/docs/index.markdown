---
layout: page
title: "Documentation"
comments: false
sharing: true
footer: true
---

<p class="warning"><strong>Word to the wise:</strong> These gems have been tested, but not extensively. If you're thinking of using SciRuby to write mission critical code, such as for driving a car or flying a space shuttle, you may wish to choose other software (for now).</p>

<strong>You might also check out the [NMatrix wiki](https://github.com/SciRuby/nmatrix/wiki/) if you have trouble finding answers here.</strong>

<h3>General instructions</h3>

These instructions are primarily for NMatrix installation. Until a new version of SciRuby is released, you'll need to install
component gems manually (see below).

1. Make sure you have *Ruby 1.9.2 or higher*. Individual components <em>may</em> work with Ruby 1.8, but we may not be able to help you if you have trouble.

2. For NMatrix, you'll also need a compiler which supports C++0x or C++11, such as GCC 4.3 or higher. [Mac OSX GCC instructions are here.](/docs/installation/mac.html)

3. [Install ATLAS](/docs/installation/atlas.html) (required for NMatrix). If you're using a Mac, this should be part of [Developer Tools](https://developer.apple.com/technologies/tools/).

4. Install NMatrix:

<pre><code>gem install nmatrix
</code></pre>

That step should take about twenty minutes and won't offer much feedback.

If that doesn't work, you might also try compiling the sources manually:

<pre><code>git clone https://github.com/SciRuby/nmatrix.git
cd nmatrix/
bundle install
rake compile
rake repackage
gem install pkg/nmatrix-*.gem
</code></pre>

Note that the above will give you the development version, which probably hasn't been released yet.

<h3>Platform notes</h3>

* [Mac OSX](/docs/installation/mac.html)
* [Linux](/docs/installation/linux.html)

<h3>Alpha workaround instructions</h3>

If you have any trouble getting SciRuby to work and are too impatient to seek assistance, most of the SciRuby components are already available as individual packages. We recommend you try out the following component gems:

<ul>
<li><em>Visualization:</em> <span class="gem"><a href="https://github.com/clbustos/rubyvis">rubyvis</a></span></li>
<li><em>Statistics:</em> <span class="gem"><a href="https://github.com/clbustos/statsample">statsample</a></span></li>
<li><em>Numeric:</em> <span class="gem"><a href="https://github.com/clbustos/minimization">minimization</a></span>, <span class="gem"><a href="https://github.com/clbustos/integration">integration</a></span>, <span class="gem"><a href="https://github.com/SciRuby/nmatrix">nmatrix</a></span></li>
<li><em>Probability:</em> <span class="gem"><a href="https://github.com/clbustos/distribution">distribution</a></span></li>
</ul>

<h4>SciRuby</h4>

<strong>Don't do this.</strong> SciRuby depends on statsample, which requires NArray, which conflicts with NMatrix.

But if you absolutely must, you can install the SciRuby gem as follows:

<pre><code>gem install gtksourceview2 # optional, for code editor
gem install statsample-optimization # optional: rb-gsl, statistics2 -- for the statsample gem
gem install sciruby
</code></pre>

We expect it to be fixed once Ruby/GSL includes NMatrix support.

Project Status
--------------
As we are currently alpha, documentation is a work in progress. If you want to get involved, please drop by our [Google Group](http://groups.google.com/group/sciruby-dev) or our IRC channel, <tt>#sciruby</tt>.

In the meantime, documentation for existing gems is scattered. You can look through the links below for additional information.

Project Pages
-------------
* [Rubyvis](http://rubyvis.rubyforge.org)
* [Statsample](http://ruby-statsample.rubyforge.org/)

rdocs
-----
* [Rubyvis](http://rubydoc.info/gems/rubyvis/)
* [Distribution](http://rubydoc.info/gems/distribution/) (part of Statsample)
* [Statsample](http://rubydoc.info/gems/statsample/)

Other resources
---------------
For SciRuby project documentation, you should visit the [SciRuby wiki](https://github.com/sciruby/sciruby/wiki).

You should subscribe to our [blog](http://www.sciruby.com/blog) if you want updates, too.
