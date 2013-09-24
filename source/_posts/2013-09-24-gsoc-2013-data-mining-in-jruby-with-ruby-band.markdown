---
layout: post
title: "GSOC 2013: Data mining in JRuby with Ruby Band"
date: 2013-09-24 10:44
comments: true
author: Alberto Arrigoni
categories: [GSOC2013,GSOC,Weka,Data mining,ruby-band,JRuby]
---
<p class="note"><strong>Editor's Note:</strong> This is the first of four blog posts detailing
our Google Summer of Code 2013 students' work.</p>

In the context of the Google Summer of Code 2013, I have developed a Ruby gem called [Ruby Band](https://github.com/arrigonialberto86/ruby-band)
that makes some selected Java software for data mining and machine learning applications available to the JRuby/Ruby
users. This project complements existing software already developed for SciRuby by adding support for the Weka Toolkit
and general functions included in the Apache Commons Math library.

As Weka does, Ruby Band features a comprehensive collection of data preprocessing and modeling techniques, and supports
several standard data mining tasks, more specifically: data pre-processing (filtering), clustering, classification,
regression, and feature selection.

All of Ruby Band's techniques have been built on the assumption that the data is
available as a single flat file or relation, where each datum is described by a fixed number of attributes. The
loaded datasets are stored in Weka Instances objects, which are used as 'core' data types for the interactions with
other software (Apache Commons Math) or platforms.

Originally, Ruby Band was called Ruby Mining. I renamed it Ruby Band, as I imagine
different software sources (Weka, Apache, etc.) working as a whole, like in a real band. Ruby Band has been designed
with a modular structure, so that it can be easily extended and integrated with other Java software. The Core module is
allows data types from different sources to be defined and integrated using converter methods;
functionalities from each piece of additional software are independently imported. This structure increases the
extensibility of the product, as in the future other developers may add functionalities according to their needs.

Though Ruby Band provides full support for the greatest part of the Weka APIs, some topics still need to be addressed
properly. As I coded, I utilized the Weka Java APIs documentation as my roadmap; if you want to contribute,
[go see what is still missing](http://weka.wikispaces.com/Use+WEKA+in+your+Java+code). The best and easiest way
to introduce a new functionality into the Ruby Band framework is to write up a pertinent Cucumber test, as a number of
Weka recipes have been posted online by the creators.

The beta version of Ruby Band has already been released for general use on Rubygems (`gem install ruby-band`).

This is a quick example of how to train a classifier on a dataset parsed from an ARFF file:

    require 'ruby-band'

    # parse a dataset
    dataset = Core::Parser.parse_ARFF(my_file.arff)

    # initialize and train a classifier
    classifier = Weka::Classifier::Lazy::KStar::Base.new do
      set_options '-M d'
      set_data dataset
      set_class_index 4
    end

    # cross-validate the trained classifier
    puts classifier.cross_validate(3)
