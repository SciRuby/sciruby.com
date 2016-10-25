---
layout: post
title:  "GSoC 2016 : A Look at SpiceRub::Body"
date:   2016-08-21 19:00:35 +0530
categories: gsoc
---

One of the main goals of this project was to make the ephemerides functions easily available to an end user. Ephemerides basically refers to an object in space whose motion is being tracked and observed. SPICE required you to provide the following parameters to observe a body in space.

`Target` : The body of interest

`Frame` : A rotational frame of reference (Default is J2000 [Not to be confused with the J2000 epoch])

`Observer` : An observing body whose viewpoint is used to chart the vector 

`Epoch` : An epoch in Ephemeris Time 

SPICE has a certain integer convention for the kind of bodies that it has support for. Each body can be referenced via a string or an integer id. While there isn't an actual strict range for integer ID classification , it is mentioned [here][int_id] and can be summed up in the following `if` and `elsif` clauses. (In Ruby constant strings are better off as symbols, so the constructor takes either an integer ID of a string symbol)

```ruby
  if body_id > 2000000
    :asteroid
  elsif body_id > 1000000
    :comet
  elsif body_id > 1000
    :body
  elsif body_id > 10
    body_id % 100 == 99 ?
      :planet : :satellite
  elsif body_id == 10
    :star
  elsif body_id > 0
    :barycenter
  elsif body_id > -1000
    :spacecraft
  elsif body_id > -100000
    :instrument
  else
    :spacecraft
```

It was very tempting to involve inheritance and extend a base Body class onto these potential classes, but I simply did not see the need for it at this point. The way it is at the moment, the `Body` object has a reader attribute type that stores some metadata about the body for the user's convenience. Perhaps as coverage of SPICE improves, this minor thing can be changed later on. 

To create a Body object, you instantiate with either a body name or a body id. Certain bodies such as instruments will require additional kernels to be loaded. To proceed seamlessly, load a leap seconds kernel, a planetary constants kernel, and an ephemeris kernel. (All avaialable in `spec/data/kernels`)

```ruby
SpiceRub::Body.new(399)
=> #<SpiceRub::Body:0x00000002769da8
     @code=399,
     @frame=:J2000,
     @name=:earth,
     @type=:planet>
 
SpiceRub::Body.new(:earth)
=> #<SpiceRub::Body:0x000000026c73c8
   @code=399,
   @frame=:J2000,
   @name=:earth,
   @type=:planet>

SpiceRub::Body.new(:moon)
=> #<SpiceRub::Body:0x0000000214ac88
   @code=301,
   @frame=:J2000,
   @name=:moon,
   @type=:satellite>
```

`399` and `:earth` map to the same body in SPICE data. The frame of reference can also be specified as a named parameter during instantiation to set a custom default frame for that particular object.

```ruby
SpiceRub::Body.new(399, frame: IAU_EARTH)
=> #<SpiceRub::Body:0x000000020b1df8
     @code=399,
     @frame=:IAU_EARTH,
     @name=:earth,
     @type=:planet>

```

In SPICE, a `state` is a 6 length column vector that stores position and velocity in 3D cartesian co-ordinates

As a base case, let's find out the the position of the Earth with respect to itself. 

```ruby

earth.position_at(SpiceRub::Time.now, observer: earth)
=> 
[
  [0.0]   [0.0]   [0.0] 
]
```

The origin as seen from itself is still the origin, so this makes sense. The methods `#velocity_at` and `#state_at` take an identical set of parameters. While there is a bit of redundancy going on, splitting them makes the API more elegant, but the basic relationship between these three vectors is the following :- 

```ruby

state = [ 
          position[0],position[1],position[2],  
          velocity[0],velocity[1],velocity[2]
        ]
```

One thing to note is that State/Velocity/Position vectors will always be returned as an `NMatrix` object, SciRuby's numerical matrix core, to allow for calculations via the NMatrix API. 

As an example that is used in the code, 1 line can turn a position vector into distance from origin. (Using Euclidean distance)

```ruby
  position = earth.position_at(SpiceRub::Time.now, observer: observer)
  Math.sqrt( (position ** 2).sum[0] )      
```

This basically performs the computation, `SquareRootOf( x ^ 2 + y ^ 2 + z ^ 2 )` Which becomes the basis for the method, `#distance_from` . 

As a simple imprecise experiment, let's find out how the speed of light can be "estimated" up with this data.

```ruby
distance = moon.distance_from(earth, now)
=> 367441.0260814745

time = moon.light_time_from(earth, now)
=> 1.2256513340354764

distance / time
=> 299792.458
```

The unit of distance here is `Km`, so the speed of light by this measurement is about pretty close to the textbook figure of `3 x 10 ^ 8  m/s`.

There is also a function to check if a list of bodies are within a radial proximity from an observing body. We already calculated the distance of the moon to be about 367k Km. The function `within_proximity` returns a list of all bodies that are within the specified radial distance from the calling body object.

```ruby

#assuming venus and mercury are instantiated

earth.within_proximity([moon, venus, mercury], 400000, now)
=> [#<SpiceRub::Body:0x0000000191c4f8
  @code=301,
  @frame=:J2000,
  @name=:moon,
  @type=:satellite>]
```

Now that we've come to the end of the functionality, I would like to state that there is another named argument `aberration_correction:` which is basically an error reduction method to provide a more accurate result than the default observation. The default `:none` option for aberration correction basically provides the geometric observations without any corrections for reception or transmission of photons. For a list of various aberration correction methods available, have a look at the documentation for [spkpos_c][spkpos] to find out if you need an aberration correction on SPICE data.

```ruby
d1 = moon.distance_from(earth, SpiceRub::Time.now, aberration_correction: :none)
=> 369111.0550333138
d2 = moon.distance_from(earth, SpiceRub::Time.now, aberration_correction: :LT)
=> 369146.60640691273

d2 - d1
=> 35.55137359892251
```

If you want to look at it another way, no aberration correction would give you the textbook response of rigid geometry, while introducing an aberration correction would give you a somewhat more realistic output accounting for the errors that do happen when these observations are made.

Finally, if you need to generate a continuous time series for a body, then `SpiceRub::Time` has two functions to aid in that

```ruby
SpiceRub::Time.linear_time_series(now, now + 86400, 4)
=> [
    #<SpiceRub::Time:0x00000001fe8b60 @et=525180780.18277323>,
    #<SpiceRub::Time:0x00000001fe8a20 @et=525209580.18277323>,
    #<SpiceRub::Time:0x00000001fe88b8 @et=525238380.18277323>,
    #<SpiceRub::Time:0x00000001fe8750 @et=525267180.18277323>
   ]
```

In this case, I took a start time and an end time that was one day after and requested 4 linearly spaced epochs. This is basically an interface to `NMatrix.linspace`. 

The other function requires you to input a start time and an end time and a step size that keeps getting added to the start time till the end time is reached. As a contrived example, we'll take two epochs, 5 days apart and ask for a step size of a day, expecting 6 elements.

```ruby

SpiceRub::Time.time_series(now, now + 5 * 86400, step: 86400)
=> [
    #<SpiceRub::Time:0x00000001646580 @et=525180780.18277323>,
    #<SpiceRub::Time:0x00000002f315b8 @et=525267180.18277323>,
    #<SpiceRub::Time:0x00000002f31590 @et=525353580.18277323>,
    #<SpiceRub::Time:0x00000002f31568 @et=525439980.18277323>,
    #<SpiceRub::Time:0x00000002f31540 @et=525526380.18277323>,
    #<SpiceRub::Time:0x00000002f31518 @et=525612780.18277323>
   ]

```

And that's it for this blog post. I would appreciate any feedback regarding this as I've been juggling the design back and forth very frequently. There is large potential of expansion of the `Body` class, particularly creating new classes as when different Bobdy objects would have a corresponding function. (For example, the function `getfov_c` which returns the field of view of an instrument could be an instance function attached to the `Instrument` subclass of `Body`, but this is just potential expansions in the future.)




[spicerub]: https://github.com/gau27/spice_rub
[readme]: https://github.com/gau27/spice_rub/blob/master/README.rdoc
[toolkit]: https://naif.jpl.nasa.gov/naif/toolkit_C.html
[time_paradox]: https://en.wikipedia.org/wiki/Year_2038_problem
[unitim]: https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/cspice/unitim_c.html
[str2et]: https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/cspice/str2et_c.html
[int_id]: https://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/req/naif_ids.html
[spkpos]: ftp://naif.jpl.nasa.gov/pub/naif/toolkit_docs/C/cspice/spkpos_c.html
