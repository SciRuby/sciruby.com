---
layout: post
title:  "GSoc 2016 : A Look at SpiceRub::Time"
date:   2016-08-21 17:00:35 +0530
categories: gsoc
---

Many popular programming languages these days ship with powerful Time classes/interfaces to make the inevitable task of dealing with time related computations a more DRY experience. Humans like their time representations to be stringy concatenations of various numbers and alphabets that enable a large variety of date and time formats to mean the same thing and still make instant sense to the human eye. It was the creation of time zones that made our convenient 24 hour clocks stay the way they are without experiencing day and night at the same "time" across the globe. So now `00:00 A.M. UTC` is `5:30 A.M. IST` for me, and the world remains sane. 

But what happens when you accept the fact that you're just a speck of micro-dust adjusting time relatively for an only slightly bigger speck of dust floating in the universe? 24 hours in a day and thus we reset after 2300, but consider this ; how would a resident of Venus know when tea-time is on Venus if he had an Earth wristwatch that reset after 24 hours? Barely a tenth of Venus' day is complete in that time! (If you know anybody intent on relocating to Mars, do not gift them a clock/watch)

So a decimal floating point representation must be the answer for uniformity. Time zones can be dealt with, we'll just pick a convenient point in time and count the seconds from there onwards so that the location on Earth doesn't matter henceforth. It'll drive humans insane with the arithmetic but machines will work just fine with this. This sort of a time system is called epoch time. 

And so the internal time of most Unix machines is the number of seconds after Midnight on `Thursday, 1 January 1970 UTC`. ( And this very convention is going to open a can of worms by [2038][time_paradox] if there is even a small set of critical machines that haven't moved on from 32 bit architectures)

But we're still not okay universally. Try going on an infinite journey to space and you'll find that counting seconds leads to some inconsistencies with your local time when you try to synchronize with Earth. How can the number of seconds after January 1970 be different in any case? Well your MacBook Pro has not been adjusted for ... relativity! Gravity bends light and thus the perception of time. There's a lot more mass, and thus a lot more gravitatonal fields in neighborhoods away from Earth. The exact details of how this works is beyond the scope of this blog. 

If the past few paragraphs were incessant and seemingly irrelevant, they were there to drive home the point that Earth time simply will not do when we step out of the ghetto to see what's happening. But Astronomy's been around for way longer, and astro-physicists came forth with a time system adjusted for the relativity effects of the solar system, called Barycentric Dynamical Time, or `TDB`. Like our machines, it counts the seconds after a certain known reference time point, except that it adjusts for relativity and can become a standard for astronomical time. 

There are many similar time scales like this, but SPICE has chosen to use `TDB` as the standard for most of their design. Within the SPICE API, TDB is the same as `Ephemeris Time` which is the main system used to specify time points of astronomical bodies. Even though Spacecrafts have their clocks co-ordinated with UTC on Earth, you would require that time in `Ephemeris Time` to be able to calculate its position and velocity using SPICE. `SpiceRub::Time` is built for this very purpose, to revolve around a main attribute `@et` for `Ephemeris Time` and provide many methods to convert to and from. 

If you'd like to proceed with the examples, you'd need a `Leap Second Kernel` file to use `SpiceRub::Time` . This is a generic kernel so you can easily use `naif0011.tls` in `spec/data/kernels` of the repository folder.

So Ephemeris Time is the number of seconds elapsed after `Noon January 1, 2000, TDB`. This point in time is also known as the `J2000` epoch. We find that out in an instant by using the `Time.parse` function which is a wrapper function for SPICE's `str2et_c` that converts many formats of strings to `Ephemeris Time`. You can have a look at the various string formats supported in its documentation [here][str2et]

```ruby
 SpiceRub::Time.parse("12:00 Jan 1 2000 TDB")
=> #<SpiceRub::Time:0x0000000325b1f8 @et=0.0>
```

So as a base case, using the reference epoch gives us 0 seconds as we would expect. Now would also be a good time to find out the discrepancy in `UTC` as well.

```ruby
SpiceRub::Time.parse("12:00 Jan 1 2000 UTC")
=> #<SpiceRub::Time:0x000000031c28e0 @et=64.18392728473108>
```

So right away we know that UTC was 64ish seconds off from TDB / ET at the time of the reference J2000 epoch. What would the difference be around right now?

```ruby
now = SpiceRub::Time.now
=> #<SpiceRub::Time:0x000000030bf8f8 @et=525173312.1827749>

(now - now.to_utc).to_f
=> 68.18277490139008

```

Well, here's a surprise, it's 68.18 now. Before I explain why that is, a brief overview of what the above code does :-

`Time.now` is a convenient way to specify your current UTC timezone. It uses Ruby's core `Time.now` method so this method is only good if you're working in UTC or Earth like Timezones. For a similar purpose, the function `Time.from_time` let's you create a SpiceRub Time object from a Ruby Time object.

The `+/-` operators return a new Time object where the right operand is added/subtracted to the left operand's `@et` when it is a float or integer. If a Time object is supplied , then it does the same with the right operand's ephemeris time instead. (Note that there really isn't a significant meaning to having a Time object whose @et is the difference/sum of two other epochs, however you can increase a certain epoch or decrease it by a constant offset of seconds) 

In our case we used `#to_utc` to convert from ephemeris time to UTC, and then the minus operator gave us a Time object that wasn't really an epoch, but a difference of two epochs, so using `#to_f` got us exactly that. 

It appears that UTC has changed by 4 seconds since 2000 with respect to ephemeris time. This is actually the adjustment of "leap seconds" that gets added to UTC to prevent it from falling too far behind other time systems. (Humans really like to hack everything, don't they?)

To verify this yourself, open up the kernel `naif0011.tls` in your text editor and Ctrl-F for `DELTET/DELTA_AT` you'll find a list like representation of the following sort :- 

```ruby
DELTET/DELTA_AT        = ( 10,   @1972-JAN-1
                           ..,   ...........
                           32,   @1999-JAN-1
                           33,   @2006-JAN-1
                           34,   @2009-JAN-1
                           35,   @2012-JUL-1
                           36,   @2015-JUL-1 )
```

Here you can see that just before the year 2000, there were 32 leap seconds added to UTC , and in 2015 when the last leap second was added, there were 36. It's an ongoing and indefinite process and so there really is no way to account for leap second errors far in the future for leap seconds that are yet to be added. As of now, the next scheduled addition is in December, 2016.

Coming back to our Time object, let's look at its basic construction. One tricky task in the API was the option to specify different epochs of reference in different time scales, like International Atomic Time. As of now, `Time.new` requires that you have kept your word of using the J2000 epoch and allows you to use a named parameter `seconds:` for specifying the time scale. The use of `scale:`was avoided as it sometimes is also used to refer to the reference epoch used.

```ruby

epochs = [:utc, :tdb, :tai].map 
  { |scale| SpiceRub::Time.new(0, seconds: scale) }

=> [#<SpiceRub::Time:0x00000002756fc8 @et=64.18392728466942>,
    #<SpiceRub::Time:0x00000002756eb0 @et=0>,
    #<SpiceRub::Time:0x00000002756cf8 @et=32.18392727400827>]
```

`:tai` here refers to International Atomic Time. For a list of more parameters and their keyword abbreviations, have a look at [this][unitim] SPICE documentation for the function that the conversion is wrapped on top of. 

But there is also a way to reference other epochs without doing the manual conversions yourself, you can call the class method `Time.at` to perform the same function as the constructor, with the option of a different reference epoch. The resultant Time object will however have its internal time referring to J2000.

A more readable way would involve step by step calculations, but that would consume runtime resources everytime `Time.at` is called, so I've basically pre-calculated the ephemeris times of the reference epochs and subtracted them from the epoch.

```ruby
  case reference.downcase
  when :j2100
    new(offset + 3155760000.0, seconds: seconds)
  when :j2000, :et
    new(offset, seconds: seconds)      
  when :j1950
    new(offset - 1577880000.0, seconds: seconds)
  when :j1900
    new(offset - 3155760000.0, seconds: seconds)
  when :gps
    new(offset - 630763148.8159368, seconds: seconds)
  when :unix
    new(offset - 946727958.8160644, seconds: seconds)
  end
```

To quickly verify the last one with the `#to_s` method

```ruby
SpiceRub::Time.new(-946727958.8160644).to_s
=> "Thu Jan 01 00:00:00 UTC 1970"
```

It's exactly the unix epoch! Let's try out 1 day (86400 seconds) after this epoch

```ruby
SpiceRub::Time.at(86400, :unix).to_s
=> "Thu Jan 01 23:59:59 UTC 1970"
```

Just a second short of heading into the next day, because we've added 86400 TDB seconds and converted the time into a UTC string.

There are some more functions provided to work in tandem with the `Body` class that I'll explain more about in the next blog post, but this more or less covers the core of `SpiceRub:Time`. Till then, thanks for reading :)

[spicerub]: https://github.com/gau27/spice_rub
[readme]: https://github.com/gau27/spice_rub/blob/master/README.rdoc
[toolkit]: https://naif.jpl.nasa.gov/naif/toolkit_C.html
[time_paradox]: https://en.wikipedia.org/wiki/Year_2038_problem
[unitim]: https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/cspice/unitim_c.html
[str2et]: https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/cspice/str2et_c.html