---
layout: post
title: "Statistics with Ruby: Time Series and General Linear Models"
date: 2013-11-07 13:15
author: Ankur Goel
comments: true
categories: [GSOC2013,GSOC,Statistics,Statsample,General Linear Models,Time Series,Kalman Filter]
---

<p class="note"><strong>Editor's Note:</strong> This is the third of four blog posts detailing our Google Summer of
Code 2013 students' work, edited by John Woods.</p>

<p class="note"><strong>Gem Maintainer's Note:</strong> These gems have changed recently. Edits reflect the changes.</p>

Introduction
------------
[Statsample](https://github.com/clbustos/statsample/) is a basic and advanced statistics suite in Ruby. It attempts to support JRuby and MRI/YARV equally, and also provides pure Ruby implementations for many functions.

Statsample is a ruby gem for statistical analysis in ruby.

It includes a rich API, except for problems involving time series and generalized linear models (GLM), for which the functionality was rather basic.

So, in this [Google Summer of Code 2013](https://www.google-melange.com/gsoc/homepage/google/gsoc2013) program, working on the SciRuby Project, I released two extensions:

* [Statsample TimeSeries](http://github.com/sciruby/statsample-timeseries)
* [Statsample GLM](https://github.com/sciruby/statsample-glm)

These gems aim to take Statsample further and incorporate various functionalities and estimation techniques on continuous data.

Statsample TimeSeries
---------------------

[*Statsample TimeSeries*](https://rubygems.org/gems/statsample-timeseries) is equipped with a variety of operations. A few of those functionalities are:

* _[Autocorrelation](http://en.wikipedia.org/wiki/Autocorrelation) of series: For finding repeating patterns (like a periodic signal) in noisy data or for identifying persistence (if it rained today, will it rain tomorrow?).
* [Autoregressive and Moving Average](http://en.wikipedia.org/wiki/Autoregressive_moving_average_model): Autoregressive models (AR and ARMA) are useful for describing random processes such as found in nature and economics believed to be
predictable from past behavior (e.g., El Niño, the stock market).
* [Partial autocorrelation](http://en.wikipedia.org/wiki/Partial_autocorrelation_function) with Yule&ndash;Walker, a method for calculating the coefficients of autoregressive models.
* [Levinson&ndash;Durbin](http://en.wikipedia.org/wiki/Levinson_recursion) estimation: for [solving linear equations](http://www.mathworks.com/help/dsp/ref/levinsondurbin.html) involving a [Toeplitz matrix](http://en.wikipedia.org/wiki/Toeplitz_matrix), such as in signal processing or cyclic signals.
* [Kalman filtering](http://en.wikipedia.org/wiki/Kalman_filter) (or linear quadratic estimation): often used for determining position and motion of a moving object based on sensor information (e.g., for drawing a vehicle's position on a map using GPS data, or for aircraft or spacecraft navigation based on sensor inputs)

To get your hands dirty, 

* Install Statsample with `gem install statsample`.
* Next, install the TimeSeries extension with `gem install statsample-timeseries`.

EDIT: Statsample-timeseries now uses [daru](www.github.com/v0dro/daru/) for data storage and cleaning. Thus all ephemeral time series statistics functions (moving average, acf, etc.) have been moved to Daru::Vector, which can be indexed on a DateTimeIndex, which lets you access data indexed by a time stamp. See the [daru README](https://github.com/v0dro/daru/blob/master/README.md) for examples.

`Statsample::TimeSeries::Series` has been deprecated in favour of `Daru::Vector`.

To demonstrate:
```ruby

require 'daru'
require 'statsample-timeseries'

ts = Daru::Vector.new(100.times.map { rand(100) }, index: Daru::DateTimeIndex.date_range(:start => '2012-2', :periods => 100))
ts.acf # Calculate auto-correlation
ts.pacf # Calculate partial autocorrelation
# Partial autocorrelation with 11 lags by maximum likelihood estimation
ts.pacf(11, 'mle') 
ts.ar # Autoregressive coefficients

# ARIMA(2, 1, 1)
k_obj = Statsample::TimeSeries.arima(ts, 2, 1, 1)
k_obj.ar # autoregressive coefficients
k_obj.ma # moving average coefficients

```

Statsample GLM
--------------
[*Statsample GLM*](https://rubygems.org/gems/statsample-glm) includes many helpful regression techniques, which can be used for regression analysis on data.
Some of those techniques are:  

* [Poisson Regression](http://en.wikipedia.org/wiki/Poisson_regression): used to model contingency tables and counts
* [Logistic Regression](http://en.wikipedia.org/wiki/Logistic_regression)
* Exponential Regression: one case of [nonlinear regression](http://en.wikipedia.org/wiki/Nonlinear_regression) (examples might include the [temperature of a cup of coffee](http://mathbits.com/MathBits/TISection/Statistics2/exponential.htm) left in a cold room, or the decay of an orbit)
* [Iteratively Reweighted Least Squares](http://en.wikipedia.org/wiki/Iteratively_reweighted_least_squares): used to mitigate the effects of outliers

The top level module for regression techniques is `Statsample::GLM`.

Using it is as simple as ever:

* First, install `statsample` by `gem install statsample`.
* Then, install GLM by `gem install `statsample-glm`.

Let's get started:

```ruby

require 'daru'
require 'statsample-glm'
# Create the datasets:
x1 = Daru::Vector.new([0.537322309644812,-0.717124209978434,-0.519166718891331,0.434970973986765,-0.761822002215759,1.51170030921189,0.883854199811195,-0.908689798854196,1.70331977539793,-0.246971150634099,-1.59077593922623,-0.721548040910253,0.467025703920194,-0.510132788447137,0.430106510266798,-0.144353683251536,-1.54943800728303,0.849307651309298,-0.640304240933579,1.31462478279425,-0.399783455165345,0.0453055645017902,-2.58212161987746,-1.16484414309359,-1.08829266466281,-0.243893919684792,-1.96655661929441,0.301335373291024,-0.665832694463588,-0.0120650855753837,1.5116066367604,0.557300353673344,1.12829931872045,0.234443748015922,-2.03486690662651,0.275544751380246,-0.231465849558696,-0.356880153225012,-0.57746647541923,1.35758352580655,1.23971669378224,-0.662466275100489,0.313263561921793,-1.08783223256362,1.41964722846899,1.29325100940785,0.72153880625103,0.440580131022748,0.0351917814720056, -0.142353224879252])
x2 = Daru::Vector.new([-0.866655707911859,-0.367820249977585,0.361486610435,0.857332626245179,0.133438466268095,0.716104533073575,1.77206093023382,-0.10136697295802,-0.777086491435508,-0.204573554913706,0.963353531412233,-1.10103024900542,-0.404372761837392,-0.230226345183469,0.0363730246866971,-0.838265540390497,1.12543549657924,-0.57929175648001,-0.747060244805248,0.58946979365152,-0.531952663697324,1.53338594419818,0.521992029051441,1.41631763288724,0.611402316795129,-0.518355638373296,-0.515192557101107,-0.672697937866108,1.84347042325327,-0.21195540664804,-0.269869371631611,0.296155694010096,-2.18097898069634,-1.21314663927206,1.49193669881581,1.38969280369493,-0.400680808117106,-1.87282814976479,1.82394870451051,0.637864732838274,-0.141155946382493,0.0699950644281617,1.32568550595165,-0.412599258349398,0.14436832227506,-1.16507785388489,-2.16782049922428,0.24318371493798,0.258954871320764,-0.151966534521183])

y = Daru::Vector.new([0,0,1,0,1,1,1,1,0,1,1,1,1,0,1,0,1,1,0,1,0,1,1,1,1,0,0,1,1,0,0,1,0,0,1,1,0,0,1,1,0,1,1,1,1,0,0,0,1,1])

x = Daru::DataFrame.new({"x1"=>x1,"x2"=>x2})

obj = Statsample::GLM.compute(x, y, :binomial)
# => Returns logistic regression object
```
The [documentation and API details is available here](http://rubydoc.info/gems/statsample-glm/Statsample/Regression)

We have some more plans for GLM module. First in the list is to make the algorithms work with singular value decomposition, because manual inversion of matrices is not fun for larger values in a Poisson regression.

Conclusion
----------

I have [blogged about most of the functionalities](http://ankurgoel.com); additional information is available there.

For more updated use cases refer to the notebooks in the respective project READMEs.

Please explore and use the libraries; I eagerly await your input, suggestions and questions. Feel free to leave any questions on [the Statsample GLM tracker](http://github.com/SciRuby/statsample-glm/issues) or [the Statsample TimeSeries tracker]([the Statsample GLM tracker](http://github.com/SciRuby/statsample-glm/issues).

I had an amazing summer!

Stay tuned and Enjoy.
