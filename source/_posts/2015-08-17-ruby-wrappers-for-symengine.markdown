---
layout: post
title: Ruby Wrappers for SymEngine, a C++ symbolic manipulation library
date: 2015-17-08 14:30
author: Abinash Meher
comments: true
categories: [SymEngine,GSOC2015,GSOC,Students]
---
A symbolic manipulation library is indispensable for scientists and students. Ruby is gaining huge popularity over the years, and a symbolic manipulation library gem  like this project in Ruby might prove to be the foundation for a computer algebra system in Ruby. With many efforts like these, Ruby might become the first choice for academicians given how easy it is to code your logic in Ruby.

The motivation for [SymEngine](https://github.com/sympy/symengine) itself was to develop it once and then extend it to other languages rather than doing the same thing all over again for a language that it is required in.

I have developed a [few notebooks](https://github.com/sympy/symengine/tree/master/symengine/ruby/notebooks) that demonstrate the use of the new SymEngine module in ruby. In the rest of the post, I would like to summarise what I did in the summer as part of [Google Summer of Code 2015](https://www.google-melange.com/gsoc/homepage/google/gsoc2015).

## Pre-midterm Evaluations

I am a newbie when it comes to ruby, and it took me a while to setup the gem and, configure files for the building of extensions. I followed the naming conventions for the project structure and the extensions, as best as I could.

### The struggle between shared, static and dynamic libraries
I faced a lot of problem in the early stages, when I was trying to build the extensions. [Ondrej](https://github.com/certik), my mentor, and [Isuru](https://github.com/isuruf), a fellow GSoC student, helped me a lot. There were many c flags that were being reported as missing. Some flags `cmake` added by default but `extconf.rb` didn't, the same one that was required to be added to build it as a shared library. I am still confused about the details, more details are [here](http://abinashmeher999.github.io/2015/05/29/Building-the-wrappers/). Finally, the library had to be built as a dynamic one. It was resolved later by hooking the process to `cmake` rather than `mkmf`.

### Load Errors and problems in linking
Many [`LoadError`s](http://abinashmeher999.github.io/2015/06/12/The-Load-Error/) popped up, but were eventually solved. [Ivan](https://github.com/dilcom/) helped a lot in debugging the errors. In the end, it turned out to be a simple file missing in the gemspec, that was not being installed.

### Reconfiguring building
One of our aims during developing this was to get rid of unessential dependencies. The ones we already had the tools for. Like later the file `extconf.rb`, that is used to generate Makefile for the extension was removed, because that could also be done by `cmake`. Flags were added to `cmake` for building the Ruby extensions, like the flag `-DWITH_RUBY=yes`. The `Makefile` then generates the library `symengine.so` in the directory `lib/symengine`.Along with `extconf.rb`, the file `extconf.h` was also gone. Along these lines, the dependency on `rake` was also removed, and with that the `Rakefile`. Any task automation will most probably be done in python. So, the `Rake::ExtensionTask` was done by `cmake` and the `Rake::GemPackageTask` was replaced by the manual method of `gem build symengine.gemspec` and `gem install symengine-0.0.0.gem`

### Travis setup
Not many projects have travis-ci setup for multiple languages. Not even the tutorials had clearly mentioned about setting up for multiple languages. But I did know about one of them, which is Shogun, the machine-learning toolbox. I referred to their `.travis.yml` and setup it up. If something like this wouldn't have worked the plan was to manually install the required version of ruby and then execute the shell commands.

### Making a basic object
Finally, I was able to successfully build the extensions, link the extensions with the SymEngine library, load the ruby-extension library in the interpreter and successfully instantiate an object of type `Basic`.

### Inheritance and Symbols
At this time, the way inheritance works(like the sequence of formation and destruction of objects of a class that had a superclass) with the Ruby C API, was confusing for all of us. I designed an [experiment](http://abinashmeher999.github.io/2015/06/26/the-symbol-class/#inheritance-in-ruby-c-api)
to check what was actually happening. That cleared things out, and made the it easier to wrap things from now on. I also wrapped the `Symbol` class during the course.

## Post-midterm Evaluations

### Redesign of the C interface
We had to design an ugly function to wrap vector in C. That led us to redesign the C interface. This approach had no reinterpret casting that was being done earlier. Each data structure had a type that was determined at compile time. For C, it was an opaque structure, while for C++ the opaque structure declared in the shared header file was implemented in the source file that had C++ data types. This [blog post](http://abinashmeher999.github.io/2015/07/03/improving-the-c-interface/) explains it further.

### Integer and Rational
While trying to port the SymEngine classes, `Integer` and `Rational`, I had to port many methods in `Basic` before that. I also replicated the `rake` tasks in NMatrix, for detection of memory leaks, in form of bash scripts.

### Common enumeration
Since all objects in the Ruby C API are of the type `CBasic`, we needed a function that would give us the typename during the runtime for the corresponding objects to be wrapped in ruby, as an object of the correct `Class` in ruby. Since, this was achieved with `enum` in C++, the same thing could be done in C too, with all the classes written manually again. But there was no guarantee for this to be consistent, if ever the features required to be wrapped for a new language, and also manually adding the class in all the enum list everytime a new class is added was prone to errors. So, to make this DRY, we automated this by sharing the list of enums. More details for the implementation can be found [here](http://abinashmeher999.github.io/2015/07/17/common-enumeration-in-c-and-c++/).

### Class coercion and interoperability
To support interoperability with the builtin ruby types, I had to overload the methods in builtin classes earlier(this was not continued). Overriding all the existing binary operations for a ruby class to support SymEngine types, violated the open/closed principle. There was indeed another way, which is *'Class Coercion'*. It was suggested by [Isuru](https://github.com/isuruf/). After that, SymEngine types could seamlessly interoperate between the ruby types.

### Arithmetic operations
After this, all the arithmetic operations had been successfully ported. Each `Basic` object can now perform arithmetic operations with other `Basic` object(sometimes even ruby objects like `Integer`). The test file in python, that had all the corresponding test cases was ported to its RSpec counterpart.

### Substitutions
Recently I completed porting the substitutions module to the extensions(`#subs`). This feature has added a lot of convenience as now you can substitute a `SymEngine::Symbol` with some other value in an expression and then `#expand` to get the result.

### Trigonometric functions
Currently, I am working on porting the trigonometric functions in SymEngine to the extensions. This would first require to wrap the `Function` class and then the `TrigFunction` class in SymEngine.

### Integration of other Ruby gems
I also have plans to integrate the ruby bindings for `gmp`, `mpfr` and `mpc` libraries, that are already available as gems, with ruby bindings for our library. I have created an issue [here](https://github.com/sympy/symengine/issues/490). Feel free to drop any suggestions.

There is much scope for improvement in both the projects. For SymEngine, to support more features like polynomials and series-expansion in the near future, and improving the user interface and the exception handling for the extensions. In short, making the extensions more ruby-ish.

I hope more people will contribute to the project and we will give a nice symbolic manipulation gem to the Ruby community.

