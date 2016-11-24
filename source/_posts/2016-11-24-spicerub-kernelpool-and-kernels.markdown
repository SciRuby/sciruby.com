---
layout: post
title:  "GSoC 2016: SpiceRub::KernelPool and Kernels"
date:   2016-11-24 12:32:35 -0500
author: Gaurav Tamba
comments: true
categories: GSOC, GSOC2016, Students, SpiceRub, SPICE
---

So this post comes rather late, but I'd still like to talk about what I did in the first week, and a little into the second week. 
Firstly, thanks to a pull request offered by my GSoC mentor John ([@mohawkjohn][mohawkjohn]), I now have rake tasks set up properly in the repository and rake seems to be working. 

There are installation instructions in the [readme][readme] and there are already some sample Kernel files in
the spec folder to try out various routines on. Apart from cloning the repository, one additional thing you will need to do is [download the SPICE toolkit][toolkit]. You may want to keep the entire compressed file for later, but you'll only need the `cspice.a` file in the `lib/` subdirectory. After this follow the instructions in the [readme][readme] and you should be good to go. (Be sure to run `bundle install` to install any dependencies that you don't already have.)

After you're done compiling and installing, run `rake pry` in the gem root directory.

If you remember, almost any useful task involving the SPICE Toolkit is preceded by loading data through kernel files. The relevant routine to do this is called `furnsh_c()` , and the most direct way to access it through Ruby is by calling the function `SpiceRub::Native.furnsh`. (However, this is not recommended because SpiceRub has a specific Ruby class unifying all the kernel related methods, and also because SPICE maintains its own internal variables for both tracking loaded kernel files and unloading them.)

Below is a crude dependency sequence:

```ruby
SpiceRub::KernelPool.load ->  SpiceRub::Native.furnsh -> furnsh_c

Ruby Interface            ->  C Accessor              -> External library 

Ruby                      ->  Native Ruby-C API       -> Native 3rd Party C
``` 

That's the basic design of the wrapper, so here are a few simple examples on using the Kernel API. `=>` denotes the interpreted output of pry.

First of all, the main KernelPool class is a singleton class, that means it can only be instantiated with the `#instance` method and the usual `#new` is private.
Any subsequent calls to `#instance` will produce the same object.

```ruby
kernel_pool = SpiceRub::KernelPool.instance
=> #<SpiceRub::KernelPool:0x00000002db3218>

rogue_kernel_pool = SpiceRub::KernelPool.new
NoMethodError: private method `new' called for SpiceRub::KernelPool:Class
from (pry):2:in `__pry__'

rogue_kernel_pool = SpiceRub::KernelPool.instance
=> #<SpiceRub::KernelPool:0x00000002db3218>

rogue_kernel_pool == kernel_pool
=> true
``` 

This is to make sure there is only one KernelPool state being maintained at a time. Now we have a bunch of kernel files in the `spec/data/kernels` folder which we'll use for this example. There is an accessor attribute called `@path` which can be used to point to a particular folder.

```ruby 
kernel_pool.path = 'spec/data/kernels'
``` 

From here on it's required that you're running pry or irb from the gem root folder in order for the paths to match in these examples.

Now let's load a couple of kernel files, you can type `system("ls", kernel_pool.path)` into your console to get a list of all the test kernels available in that folder. The KernelPool object has a `#load` method to load kernel files. If the variable path is set, then you only need to enter the file name, otherwise the entire path needs to be provided. An integer denoting the index of the kernel in the pool is returned if the load is successful.


```ruby 
kernel_pool.load("naif0011.tls")
=> 0
```

Note that this is the same as providing the full relative path of `spec/data/kernels/naif0011.tls` when the `path`	variable is not set or nil.

Let's load two more files:-

```ruby 
kernel_pool.load("moon_pa_de421_1900-2050.bpc")
=> 1

kernel_pool.load("de405_1960_2020.bsp")
=> 2

kernel_pool
=> #<SpiceRub::KernelPool:0x00000002db3218
 @path="spec/data/kernels",
 @pool=
  [#<SpiceRub::KernelPool::SpiceKernel:0x0000000270fab0 @loaded=true, @path_to="spec/data/kernels/naif0011.tls">,
   #<SpiceRub::KernelPool::SpiceKernel:0x000000026bd698 @loaded=true, @path_to="spec/data/kernels/moon_pa_de421_1900-2050.bpc">,
   #<SpiceRub::KernelPool::SpiceKernel:0x000000024ce698 @loaded=true, @path_to="spec/data/kernels/de405_1960_2020.bsp">]>

```

So now if you try to view the `@pool` member of `kernel_pool`, you'll find three SpiceKernel objects with `@loaded=true` and their respective file paths.
There isn't much to do with a `SpiceKernel` object except unload it, or check its state. Note that you can only load kernel files into a `KernelPool` object and unload them via the `SpiceKernel` object. 

You can access the kernel pool by calling the `#[]` operator and using the index that was returned on load:

```ruby
kernel_pool[0]
=> #<SpiceRub::KernelPool::SpiceKernel:0x0000000270fab0 @loaded=true, @path_to="spec/data/kernels/naif0011.tls">

kernel_pool.count
=> 3

kernel_pool[0].unload!
=> true

kernel_pool.count
=> 2

```

So here we unload the first kernel and note that the count drops to 2. If you look up `kernel_pool[0]`, you'll find that the kernel is still present &mdash; but its `@loaded` variable has been set to false, which means that it has been removed from the CSPICE internal kernel pool.

```ruby
kernel_pool[0]
=> #<SpiceRub::KernelPool::SpiceKernel:0x0000000270fab0 @loaded=false, @path_to="spec/data/kernels/naif0011.tls">
```

To unload all kernels simultaneously and delete the kernel pool, use `#clear!`

```ruby

kernel_pool.clear!
=> true

kernel_pool.empty?
=> true

kernel_pool.pool
=> []
```

And that about wraps up this blog post on basic kernel handling. Since we know how to load data but not use it yet, I'll cover that and the various kernel types in the next blog post. Thank you for reading.



[mohawkjohn]: https://github.com/mohawkjohn
[spicerub]: https://github.com/sciruby/spice_rub
[readme]: https://github.com/sciruby/spice_rub/blob/master/README.rdoc
[toolkit]: https://naif.jpl.nasa.gov/naif/toolkit_C.html
