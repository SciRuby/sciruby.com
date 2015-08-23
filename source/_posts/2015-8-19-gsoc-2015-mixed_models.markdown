---
layout: post
title: "Statistical linear mixed models in Ruby with mixed_models (GSoC2015)"
date: 2015-08-19
comments: true
author: Alexej Gossmann
categories: [GSOC,GSOC2015,Statistics,Modeling,Regression]
---

Google Summer of Code 2015 is coming to an end. During this summer, I have learned too many things to list here about statistical modeling, Ruby and software development in general, and I had a lot of fun in the process!

## Linear mixed models

My GSoC project is the Ruby gem [mixed_models](https://github.com/agisga/mixed_models). Mixed models are statistical models which predict the value of a response variable as a result of fixed and random effects. The gem in its current version can be used to fit statistical linear mixed models and perform statistical inference on the model parameters as well as to predict future observations. A number of tutorials/examples in IRuby notebook format are accessible from the `mixed_models` [github repository](https://github.com/agisga/mixed_models).

Linear mixed models are implemented in the class `LMM`. The constructor method `LMM#initialize` provides a flexible model specification interface, where an arbitrary covariance structure of the random effects terms can be passed as a `Proc` or a block.  

A convenient user-friendly interface to the basic model fitting algorithm is `LMM#from_formula`, which uses the formula language of the R mixed models package `lme4` for model specification. With the `#from_formula` method, the user can conveniently fit models with categorical predictor variables, interaction fixed or random effects, as well as multiple crossed or nested random effects, all with just one line of code.

Examples are given in the sections below.

### Implementation

The parameter estimation in `LMM#initialize` is largely based on the approach developed by the authors of the R mixed models package `lme4`, which is delineated in the `lme4` [vignette](https://cran.r-project.org/web/packages/lme4/vignettes/lmer.pdf). I have tried to make the code of the model fitting algorithm in `LMM#initialize` easy to read, especially compared to the corresponding implementation in `lme4`. 

The `lme4` code is largely written in C++, which is integrated in R via the packages `Rcpp` and `RcppEigen`. It uses [CHOLMOD](https://developer.nvidia.com/cholmod) code for various sparse matrix tricks, and it involves passing pointers to C++ object to R (and vice versa) many times, and passing different R environments from function to function. All this makes the `lme4` code rather hard to read. Even Douglas Bates, the main developer of `lme4`, admits that ["The end result is confusing (my fault entirely) and fragile"](https://stat.ethz.ch/pipermail/r-sig-mixed-models/2014q4/022791.html), because of all the utilized performance improvements. I have analyzed the `lme4` code in three blog posts ([part 1](http://agisga.github.io/Dissect_lmer_part1/), [part 2](http://agisga.github.io/Dissect_lmer_part2/) and [part 3](http://agisga.github.io/Dissect_lmer_part3/)) before starting to work on my gem `mixed_models`.

The method `LMM#initialize` is written in a more functional style, which makes the code shorter and (I find) easier to follow.  All matrix calculations are performed using the gem [`nmatrix`](https://github.com/SciRuby/nmatrix), which has a quite intuitive syntax and contributes to the overall code readability as well.
The Ruby gem loses with respect to memory consumption and speed in comparison to `lme4`, because it is written in pure Ruby and does not utilize any sparse matrix tricks. However, for the same reasons the `mixed_models` code is much shorter and easier to read than `lme4`. Moreover, the linear mixed model formulation in `mixed_models` is a little bit more general, because it does not assume that the random effects covariance matrix is sparse. More about the implementation of `LMM#initialize` can be found in [this blog post](http://agisga.github.io/First-linear-mixed-model-fit/).

### Other existing tools 

Popular existing software packages for mixed models include the R package [`lme4`](https://cran.r-project.org/web/packages/lme4/index.html) (which is arguably the standard software for linear mixed models), the R package [`nlme`](https://cran.r-project.org/web/packages/nlme/index.html) (an older package developed by the same author as `lme4`, still widely used), Python's [`statmodels`](https://github.com/statsmodels/statsmodels/blob/master/statsmodels/regression/mixed_linear_model.py), and the Julia package [`MixedModels.jl`](https://github.com/dmbates/MixedModels.jl). 

Below, I give a couple of examples illustrating some of the capabilities of `mixed_models` and explore how it compares to the alternatives.

### A usage example and discussion

As an example, we use [data](http://archive.ics.uci.edu/ml/datasets/BlogFeedback) from the UCI machine learning repository, which originate from blog posts from various sources in 2010-2012, in order to model (the logarithm of) the number of comments that a blog post receives. The linear predictors are the text length, the log-transform of the average number of comments at the hosting website, the average number of trackbacks at the hosting website, and the parent blog posts. We assume a random effect on the number of comments due to the day of the week on which the blog post was published. In `mixed_models` this model can be fit with

```Ruby
model_fit = LMM.from_formula(formula: "log_comments ~ log_host_comments_avg + host_trackbacks_avg + length + has_parent_with_comments + (1 | day)", 
                              data: blog_data)
```

and we can display some information about the estimated fixed effects with

```Ruby
puts model_fit.fix_ef_summary.inspect(24)
```

which produces the following output:

```
                                             coef                       sd                  z_score            WaldZ_p_value 
               intercept       1.2847896684307731     0.030380582281933178        42.28983027737477                      0.0 
   log_host_comments_avg        0.415586319225577     0.007848368759350875        52.95193586953086                      0.0 
     host_trackbacks_avg     -0.07551588997745964     0.010915623834434068       -6.918146971979714    4.575895218295045e-12 
                  length   1.8245853808280765e-05    2.981631039432429e-06        6.119420400102211    9.391631916599863e-10 
has_parent_with_comments      -0.4616662830553772      0.13936886611993773      -3.3125496095955715    0.0009244972814528296 
```

We can also display the estimated random effects coefficients and the random effects standard deviation,

```Ruby
puts "Random effects coefficients:"
puts model_fit.ran_ef
puts "Random effects correlation structure:"
puts model_fit.ran_ef_summary.inspect
```

which produces

```
Random effects coefficients:
{:intercept_fr=>0.0, :intercept_mo=>0.0, :intercept_sa=>0.0, :intercept_su=>0.0, :intercept_th=>0.0, :intercept_tu=>0.0, :intercept_we=>0.0}
Random effects standard deviation:

#<Daru::DataFrame:70278348234580 @name = 8e11a27f-81b0-48a0-9771-085a8f30693d @size = 1>
                  day 
       day        0.0 
```

Interestingly, the estimates of the random effects coefficients and standard deviation are all zero!
That is, we have a singular fit. Thus, our results imply that the day of the week on which a blog post is published has no effect on the number of comments that the blog post will receive. 

It is worth pointing out that such a model fit with a singular covariance matrix is problematic with the current version of Python's `statmodels` (described as "numerically challenging" in the [documentation](http://statsmodels.sourceforge.net/devel/mixed_linear.html)) and the R package `nlme` ("Singular covariance matrices correspond to infinite parameter values", a [mailing list reply](https://stat.ethz.ch/pipermail/r-sig-mixed-models/2014q4/022791.html) by Douglas Bates, the author of `nlme`). However, `mixed_models`, `lme4` and `MixedModels.jl` can handle singular fits without problems.
In fact, like `mixed_models` above, `lme4` estimates the random effects coefficients and standard deviation to be zero, as we can see from the following R output:

```R
> mod <- lmer(log_comments ~ log_host_comments_avg + host_trackbacks_avg + length + has_parent_with_comments + (1|day), data = df)
Warning message:
Some predictor variables are on very different scales: consider rescaling 
> ranef(mod)
$day
   (Intercept)
fr           0
mo           0
sa           0
su           0
th           0
tu           0
we           0

> VarCorr(mod)
 Groups   Name        Std.Dev.
 day      (Intercept) 0.0000  
 Residual             1.2614
```

Unfortunately, `mixed_models` is rather slow when applied to such a large data set (`blog_data` is a data frame of size 22435&times;8), especially when compared to `lme4` which uses many sparse matrix tricks and is mostly written in C++ (integrated in R via `Rcpp`) to speed up computation. The difference in performance between `mixed_models` and `lme4` is on the order of hours for large data, and Julia's `MixedModels.jl` promises to be even faster than `lme4`. However, there is no noticeable difference in performance speed for smaller data sets.

[The full data analysis of the blog post data can be found in this IRuby notebook](http://nbviewer.ipython.org/github/agisga/mixed_models/blob/master/notebooks/blog_data.ipynb).

### A second example and statistical inference on the parameter estimates

Often, the experimental design or the data suggests a linear mixed model whose random effects are associated with multiple grouping factors. A specification of multiple random effects terms which correspond to multiple grouping factors is often referred to as *crossed random effect*, or *nested random effects* if the corresponding grouping factors are nested in each other.
A good reference on such models is [Chapter 2](http://lme4.r-forge.r-project.org/book/Ch2.pdf) of Douglas Bates' `lme4` book.

Like `lme4`, `mixed_models` is particularly well suited for models with crossed or nested random effects. The current release of `statmodels`, however, does not support crossed or nested random effects (according to the [documentation](http://statsmodels.sourceforge.net/devel/mixed_linear.html)).

As an example we fit a linear mixed model with nested random effects to a data frame with 100 rows, of the form:

```Ruby
#<Daru::DataFrame:69912847885160 @name = 2b161c5d-00de-4240-be50-8fa84f3aed24 @size = 5>
                    a          b          x          y 
         0         a3         b1 0.38842531 5.10364866 
         1         a3         b2 0.44622300 6.23307061 
         2         a3         b1 1.54993657 12.2050404 
         3         a3         b1 1.52786614 12.0067595 
         4         a3         b2 0.76011212 8.20054527
```

We consider the following model:

* We take `y` to be the response and `x` its predictor.
* We consider the factor `b` to be nested within the factor `a`.
* We assume that the intercept varies due to variable `a`; that is, a different (random) intercept term for each level of `a`.
* Moreover, we assume that the intercept varies due to the factor `b` which is nested in `a`; that is, different (random) intercept for each combination of levels of `a` and `b`.

That is, mathematically the model can be expressed as

```
y = beta_0 + beta_1 * x + gamma(a) + delta(a,b) + epsilon
```

where `gamma(a) ~ N(0, phi**2)` and `delta(a,b) ~ N(0, psi**2)` are normally distributed random variables which assume different realizations for different values of `a` and `b`, and where `epsilon` is a random Gaussian noise term with variance `sigma**2`. The goal is to estimate the parameters `beta_0`, `beta_1`, `phi`, `psi` and `sigma`.

We fit this model in `mixed_models`, and display the estimated random effects correlation structure with

```Ruby
mod = LMM.from_formula(formula: "y ~ x + (1|a) + (1|a:b)", 
                       data: df, reml: false)
puts mod.ran_ef_summary.inspect
```

which produces the output

```
                    a    a_and_b 
         a 1.34108300        nil 
   a_and_b        nil 0.97697500
```

The correlation between the factor `a` and the nested random effect `a_and_b` is denoted as `nil`, because the random effects in the model at hand are assumed to be independent.

An advantage of `mixed_models` over some other tools is the simplicity with which p-values and confidence intervals for the parameter estimates can be calculated using a multitude of available methods. Such methods include a likelihood ratio test implementation, multiple bootstrap based methods (which run in parallel by default), and methods based on the Wald Z statistic.

We can compute five types of 95% confidence intervals for the fixed effects coefficients with the following line of code:

```Ruby
mod.fix_ef_conf_int(method: :all, nsim: 1000)
```

which yields the result

```
                                          intercept                                        x 
    wald_z [-1.0442515623151203, 2.433416817887737]   [4.302419420148841, 5.038899876985704] 
boot_basic [-0.9676586601496888, 2.486799230544233]    [4.30540212917657, 5.028701160534481] 
 boot_norm [-1.0575520080398213, 2.4667867000424115   [4.295959190826356, 5.043382379744274] 
    boot_t [-0.9676586601496886, 2.486799230544233]    [4.30540212917657, 5.028701160534481] 
 boot_perc [-1.0976339749716164, 2.3568239157223054   [4.312618136600064, 5.035917167957975] 

```

For example, we see here that the intercept term is likely not significantly different from zero. We could proceed now by performing hypotheses tests using `#fix_ef_p` or `#likelihood_ratio_test`, or by refitting a model without an intercept using `#drop_fix_ef`.

We can also test the nested random effect for significance, in order to decide whether we should drop that term from the model to reduce model complexity. We can use a bootstrap based version of likelihood ratio test as follows.

```Ruby
mod.ran_ef_p(variable: :intercept, grouping: [:a, :b], 
             method: :bootstrap, nsim: 1000)
```

We get a p-value of 9.99e-4, suggesting that we probably should keep the term `(1|a:b)` in the model formula.

### A third example &mdash; a less conventional model fit

Another advantage of `mixed_models` against comparable tools is the ease of fitting models with arbitrary covariance structures of the random effects, which are not covered by the formula interface of `lme4`. This can be done in a user-friendly manner by providing a block or a `Proc` to the `LMM` constructor. This unique feature of the Ruby language makes the implementation and usage of the method incredibly convenient. A danger of allowing for arbitrary covariance structures is, of course, that such a flexibility gives the user the freedom to specify degenerate and computationally unstable  models.

As an example we look at an application to genetics, namely to SNP data ([single-nucleotide polymorphism](https://en.wikipedia.org/wiki/Single-nucleotide_polymorphism)) with known pedigree structures (family relationships of the subjects). The family information is prior knowledge that we can model in the random effects of a linear mixed effects model.

We model the quantitative trait `y` (a vector of length 1200) as

```
y = X * beta + b + epsilon,
```

where `X` is a `1200 x 130` matrix containing the genotypes (i.e. 130 SNPs for each of the 1200 subjects); `epsilon` is a vector of independent random noise terms with variances equal to `sigma**2`; `beta` is a vector of unknown fixed effects coefficients measuring the contribution of each SNP to the quantitative trait `y`; and `b` is a vector of random effects.

If we denote the kinship matrix by `K`, then we can express the probability distribution of `b` as `b ~ N(0, delta**2 * 2 * K)`, where we multiply `K` by `2` because the diagonal of `K` is constant `0.5`, and where `delta**2` is a unknown scaling factor.

The goal is to estimate the unknown parameters `beta`, `sigma`, and `delta`, and to determine which of the fixed effects coefficients are significantly different from 0 (i.e. which SNPs are possibly causing the variability in the trait `y`).

In order to specify the covariance structure of the random effects, we need to pass a block or `Proc` that produces the upper triangular Cholesky factor of the covariance matrix of the random effects from an input Array. In this example, that would be the multiplication of the prior known Cholesky factor of the kinship matrix with a scaling factor.

Having all the model matrices and vectors, we compute the Cholesky factor of the kinship matrix and fit the model with


```Ruby
# upper triangulat Cholesky factor
kinship_mat_cholesky_factor = kinship_mat.factorize_cholesky[0] 

# Fit the model
model_fit = LMM.new(x: x, y: y, zt: z,
                    x_col_names: x_names, 
                    start_point: [2.0], 
                    lower_bound: [0.0]) { |th| kinship_mat_cholesky_factor * th[0] }
```

Then we can use the available hypotheses test and confidence interval methods to determine which SNPs are significant predictors of the quantitative trait. Out of the 130 SNPs in the model, we find 24 to be significant as linear predictors. 

See [this blog post](http://agisga.github.io/mixed_models_applied_to_family_SNP_data/) for a full analysis of this data with `mixed_models`.

## Room for improvement and future work

* Writing the formula language interpretation code used by `LMM#from_formula` from scratch was not easy. Much of the code can be reorganized to be easier to read and to use in other projects. Possibly, the formula interface should be separated out, similar to how it is done with the Python package [patsy](https://github.com/pydata/patsy). Also, some shortcut symbols (namely `*`, `/`, and `||`) in the model specification formula language are currently not implemented. 

* I plan to add linear mixed models for high-dimensional data (i.e. more predictors than observations) to `mixed_models`, because that work would be in line with my current PhD research.

* I plan to add generalized linear mixed models capabilities to `mixed_models`, which can be used to fit mixed models to discrete data (such as binary or count data).

## Acknowledgement

I want to thank Google and the [Ruby Science Foundation](sciruby.com) for giving me this excellent opportunity! I especially want to thank [Pjotr Prins](http://thebird.nl/) who was my mentor for the project for much helpful advice and suggestions as well as his prompt responses to any of my concerns. I also want to thank my fellow GSoC participants [Will](https://github.com/wlevine), [Ivan](https://github.com/dilcom), and [Sameer](https://github.com/v0dro) for their help with certain aspects of my project.
