---
layout: post
title: Ruby Wrappers for SymEngine, a C++ symbolic manipulation library
date: 2015-08-17 14:30
author: Abinash Meher
comments: true
categories: [SymEngine,GSOC2015,GSOC,Students]
---
I am sure you have heard about [Wolfram Alpha](http://www.wolframalpha.com/). Some of you would have used its [Mathematics section](http://www.wolframalpha.com/examples/Math.html) at some point of time to cross check your solution to a maths problem. If you haven't, please do. A computer algebra system (CAS) does the same thing. Algebraic computation or symbolic computation are all used interchangeably. A CAS solves problems the same way a human does, but way more quickly and precisely. There is very less chance for an error, that we humans often make.

## Introduction
My project was to write the Ruby extensions for the library [SymEngine](https://github.com/sympy/symengine) and come up with a Ruby-ish interface, after which we can use the features of SymEngine from Ruby.

[SymEngine](https://github.com/sympy/symengine) is a library for symbolic computation in C++. You may ask, why SymEngine? There are other CASs that I know of. This question was indeed asked. At the beginning, the idea was to use ruby wrappers for [sage](http://www.sagemath.org/) (a mathematics software system) which uses [Pynac](http://pynac.org/), an interface to [GiNaC](http://www.ginac.de/) (another CAS). As it turns out from the benchmarks, SymEngine is much faster than Pynac. What about directly wrapping GiNaC? SymEngine is also a bit faster than GiNaC.

The motivation for [SymEngine](https://github.com/sympy/symengine) itself is to develop it once in C++ and then use it from other languages rather than doing the same thing all over again for each language that it is required in. In particular, a long term goal is to make Sage as well as SymPy use it by default, thus unifying the Python CAS communities. The goal of implementing the Ruby wrappers is to provide a CAS for the Ruby community.

### How will this be useful?
There are times when we might need a symbolic computation library. Here is an incomplete list of some of the situations:

- We need to do some algebra to get systems of equations in suitable form for numerical computation.
- We need to make substitutions for some variables and don’t want to risk a math error by hand. There might be times when we want to partially simplify an expression by substituting only a few of its variables.
- We have a situation where we need to find the optimum number of something to maximise our profits, and the mathematical model devised is just too complicated to solve by hand.
- We need to perform non-trivial derivatives or integrals.
- We are trying to understand the domain of a new function.
- Most root finding algorithms search for a single root, whereas often there are more than one.
- We need to solve a linear system or manipulate a symbolic matrix exactly.
- We need to manipulate exact expressions and solutions, as opposed to approximate ones using a numerical method.

With that said, a symbolic manipulation library is indispensable for scientists and students. Ruby has gained a great deal of popularity over the years, and a symbolic manipulation library gem  like this project in Ruby might prove to be the foundation for a computer algebra system in Ruby. With many efforts like these, Ruby might become the first choice for academicians given how easy it is to code your logic in Ruby.

## How to install the gem?
To install, please follow the [compile instructions given in the README](https://github.com/sympy/symengine/blob/master/symengine/ruby/README.md). After you are done, I would suggest to test the extensions. To run the test suite execute `rspec spec` on the command line, from the `symengine/ruby` dir.

The gem is still in alpha release. Please help us out by reporting any issues in [the repo issue tracker](https://github.com/sympy/symengine/issues).

## What can I do with the gem?
Currently, the following features are available in the gem:
- Construct expressions out of variables (mathematical).
- Simplify the expressions.
- Carry out arithmetic operations like `+`, `-`, `*`, `/`, `**` with the variables and expressions.
- Extract arguments or variables from an expression.
- Differentiate an expression with respect to another.
- Substitute variables with other expressions.

Features that will soon be ported to the SymEngine gem
- Functions, including trigonometric, hyperbolic and some special functions.
- Matrices, and their operations.
- Basic number-theoretic functions.

I have developed a [few IRuby notebooks](https://github.com/sympy/symengine/tree/master/symengine/ruby/notebooks) that demonstrate the use of the new SymEngine module in ruby.

Below is an example taken from the notebooks.

---
## Using the SymEngine Gem
SymEngine is a module in the extensions, and the classes are a part of it. So first you fire up the interpreter or an IRuby notebook and load the file:
```ruby
require 'symengine'
=> true
```
Go ahead and try a function:
```ruby
SymEngine.ascii_art
=>   _____           _____         _
    |   __|_ _ _____|   __|___ ___|_|___ ___
    |__   | | |     |   __|   | . | |   | -_|
    |_____|_  |_|_|_|_____|_|_|_  |_|_|_|___|
          |___|               |___|          
```
or create a variable:
```ruby
basic = SymEngine::Basic.new
=> #<SymEngine::Basic:0x00000001e95290>
```
This shows that we have successfully loaded the module.

### SymEngine::Symbol
Just like there are variables like x, y, and z in a mathematical expression or equation, we have `SymEngine::Symbol` in SymEngine to represent them. To use a variable, first we need to make a `SymEngine::Symbol` object with the string we are going to represent the variable with.:
```ruby
puts x = SymEngine::Symbol.new("x")
puts y = SymEngine::Symbol.new("y")
puts z = SymEngine::Symbol.new("z")

x
y
z
```
Then we can construct expressions out of them:
```ruby
e = (x-y)*(x**y/z)
e.to_s
=> "x**y*(x - y)/z"
```
In SymEngine, every object is an instance of Basic or its subclasses. So, even an instance of `SymEngine::Symbol` is a Basic object.:
```ruby
x.class
=> SymEngine::Symbol

x.is_a? SymEngine::Basic
=> true
```
Now that we have an expression, we would like to see it's expanded form using `#expand`:
```ruby
f = e.expand()
f.to_s
=> "x**(1 + y)/z - x**y*y/z"
```
Or check if two expressions are same:
```ruby
f == - (x**y*y/z) + (x**y*x/z)
=> true
```
But `e` and `f` are not equal since they are only mathematically equal, not structurally:
```ruby
e == f
=> false
```
Let us suppose you want to know **what variables/symbols your expression has**. You can do that with the `#free_symbols` method, which returns a set of the symbols that are in the expression.:
```ruby
f.free_symbols
=> #<Set: {#<SymEngine::Basic:0x00000001f0ca70>, #<SymEngine::Basic:0x00000001f0ca48>, #<SymEngine::Basic:0x00000001f0ca20>}>
```
Let us use `#map` method to see the elements of the `Set`.:
```ruby
f.free_symbols.map { |x| x.to_s }
=> ["x", "y", "z"]
```
`#args` returns the terms of the expression,:
```ruby
f.args.map { |x| x.to_s }
["-x**y*y/z", "x**(1 + y)/z"]
```
or if it is a single term it breaks down the elements:
```ruby
f.args[0].args.map { |k| k.to_s }
=> ["-1", "x**y", "y", "z**(-1)"]
```
### SymEngine::Integer

You can make objects of class `SymEngine::Integer`. It's like regular `Integer` in ruby kernel, except it can do all the operations a `Basic` object can &mdash; such as arithmetic operations, etc.:
```ruby
a = SymEngine::Integer.new(12)
b = SymEngine::Integer.new(64)
a**b

=> 1168422057627266461843148138873451659428421700563161428957815831003136
```
Additionally, it can support numbers of arbitrarily large length.
```ruby
(a**x).to_s
=> "12**x"
```

### SymEngine::Rational

You can also make objects of class `SymEngine::Rational` which is the SymEngine counterpart for `Rationals` in Ruby.:
```ruby
c = Rational('2/3')
d = SymEngine::Rational.new(c)

=> 2/3
```
Like any other `Basic` object arithmetic operations can be done on this rational type too.:
```ruby
(a-d).to_s
=> "34/3"
```
---
You **need not create** an instance of `SymEngine::Integer` or `SymEngine::Rational`, every time you want to use them in an expression that uses many `Integer`s. Let us say you already have `Integer`/`Rational` object. Even then you can use them without having to create a new `SymEngine` object.:
```ruby
k = (1 / (x * y) - x * y + 2) * (c + x * y) # c is a Rational object, not SymEngine::Rational
k.to_s
=> "(2/3 + x*y)*(2 + 1/(x*y) - x*y)"
```
As you can see, ruby kernel `Integer`s and `Rational`s interoperate seamlessly with the `SymEngine` objects.
```ruby
k.expand.to_s
=> "7/3 + (2/3)*1/(x*y) + (4/3)*x*y - x**2*y**2"
```
---
## What I learned

In the rest of the post, I would like to summarise my work and what I learned as a participant of [Google Summer of Code 2015](https://www.google-melange.com/gsoc/homepage/google/gsoc2015).

## Pre-midterm Evaluations

I am a newbie when it comes to Ruby, and it took me a while to setup the gem and configure files for the building of extensions.

### The struggle between shared, static and dynamic libraries
I faced a lot of problem in the early stages, when I was trying to build the extensions. [Ondrej](https://github.com/certik), my mentor, and [Isuru](https://github.com/isuruf), a fellow GSoC student, helped me a lot. There were many C flags that were being reported as missing. Some flags `cmake` added by default but `extconf.rb` didn't, the same one that was required to be added to build it as a shared library. I am still confused about the details, some of which are [explored in greater detail in my personal blog](http://abinashmeher999.github.io/2015/05/29/Building-the-wrappers/). Finally, the library had to be built as a dynamic one. The problem of missing C flags was resolved later by hooking the process to `cmake` rather than `mkmf`.

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

---
There is much scope for improvement in both the projects. For SymEngine, to support more features like polynomials and series-expansion in the near future, and improving the user interface and the exception handling for the extensions. In short, making the extensions more ruby-ish.

I am grateful to my mentor, [Mr. Ondřej Čertík](https://github.com/certik), the [Ruby Science Foundation](http://sciruby.com/) and the [SymPy Organisation](http://www.sympy.org/en/index.html) for the opportunity that they gave me and guiding me through the project, and my team-mates for helping me with the issues. I hope more people will contribute to the project and together we will give a nice symbolic manipulation gem to the Ruby community.

