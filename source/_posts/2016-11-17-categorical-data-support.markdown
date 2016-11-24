---
layout: post
title: "GSoC 2016 Summary, Adding categorical data support"
date: 2016-11-17 08:41
comments: true
Author: Lokesh Sharma
GitHub - @lokeshh
categories: [GSOC 2016, GSOC, Daru, Statsample, Statsample-GLM, Categorical data, Gruff, Nyaplot, Gnuplotrb]
---


Support for categorical data is important for any data analysis tool. This summer I implemented categorical data capabilities for:

- Convenient and efficient data wrangling for categorical data in [Daru](https://github.com/v0dro/daru)
- Visualization of categorical data
- Multiple linear regression and generalized linear models (GLM) with categorical variables in [Statsample](https://github.com/SciRuby/statsample) and [Statsample-GLM](https://github.com/SciRuby/statsample-glm)

[Here's](https://summerofcode.withgoogle.com/archive/2016/projects/5356167010189312/) my project page.

Lets talk about each of them in detail.

#### Analyzing catgorical data with Daru

Categorical data is now readily recognized by [Daru](https://github.com/v0dro/daru) and Daru has all the procedures to deal with it.

To analyze categorical variable, simply turn the numerical vector to categorical and you are ready to go.

```ruby
# The dataset
shelter_data

```
(This is animal shelter data taken from the [kaggle compeption](https://www.kaggle.com/c/shelter-animal-outcomes).)

![](http://i65.tinypic.com/xeliqs.png)

```ruby
# Tell Daru which variables are categorical
shelter_data.to_category 'OutcomeType', 'AnimalType', 'SexuponOutcome', 'Breed', 'Color'

# Or quantize a numerical variable to categorical
shelter_data['AgeuponOutcome'] = shelter_data['AgeuponOutcome(Weeks)'].cut [0, 1, 4, 52, 260, 1500],
    labels: [:less_than_week, :less_than_month, :less_than_year, :one_to_five_years, :more_than__five_years]

# Do your operations on categorical data
shelter_data['AgeuponOutcome'].frequencies.sort ascending: false
```
![](http://i67.tinypic.com/w1u3vs.png)

```ruby
small['Breed'].categories.size
#=> 1380
# Merge infrequent categories to make data analysis easy
other_cats = shelter_data['Breed'].categories.select { |i| shelter_data['Breed'].count(i) < 10 }
other_cats_hash = other_cats.zip(['other']*other_cats.size).to_h
shelter_data['Breed'].rename_categories other_cats_hash
shelter_data['Breed'].frequencies
# View the data
small['Breed'].frequencies.sort(ascending: false).head(10)
```
![](http://i64.tinypic.com/25rcu8m.png)

Please refer to [this blog post](http://lokeshh.github.io/blog/2016/06/21/categorical-data/) to know more.


#### Visualizing categorical data

With the help of [Nyaplot](https://github.com/SciRuby/nyaplot), [GnuplotRB](https://github.com/SciRuby/gnuplotrb) and [Gruff](https://github.com/topfunky/gruff), Daru now provides ability to visualize categorical data as it does with numerical data.

To plot vector with Nyaplot one needs to call the function `#plot`.

```ruby
# dv is a caetgorical vector
dv = Daru::Vector.new ['III']*10 + ['II']*5 + ['I']*5, type: :category, categories: ['I', 'II', 'III']

dv.plot(type: :bar, method: :fraction) do |p, d|
  p.x_label 'Categories'
  p.y_label 'Fraction'
end
```

![](http://i64.tinypic.com/2s6onsw.png)

Given a dataframe, one can plot the scatter plot such that the points color, shape and size can be varied acording to a categorical variable.

```ruby
# df is a dataframe with categorical variable :c
df = Daru::DataFrame.new({
  a: [1, 2, 4, -2, 5, 23, 0],
  b: [3, 1, 3, -6, 2, 1, 0],
  c: ['I', 'II', 'I', 'III', 'I', 'III', 'II']
  })
df.to_category :c

df.plot(type: :scatter, x: :a, y: :b, categorized: {by: :c, method: :color}) do |p, d|
  p.xrange [-10, 10]
  p.yrange [-10, 10]
end
```

![](http://i64.tinypic.com/2mcfx28.png)

In a similar manner Gnuplot and Gruff also supports plotting of categorical variables.

An additional work I did was to add Gruff with Daru. Now one can plot vectors and dataframes also using Gruff.

See more notebooks on visualizing categorical data with Daru [here](http://nbviewer.jupyter.org/github/SciRuby/sciruby-notebooks/tree/master/Data%20Analysis/Plotting/).

#### Regression with categorical data

Now categorical data is supported in multiple linear regression and generalized linear models (GLM) in [Statsample](https://github.com/SciRuby/statsample) and [Statsample-GLM](https://github.com/SciRuby/statsample-glm).

A new formula language (like that used in R or [Patsy](https://github.com/pydata/patsy)) has been introduced to ease the task of specifying regressions.

Now there's no need to manually create a dataframe for regression.

```ruby
require 'statsample-glm'

formula = 'OutcomeType_Adoption~AnimalType+Breed+AgeuponOutcome(Weeks)+Color+SexuponOutcome'
glm_adoption = Statsample::GLM::Regression.new formula, train, :logistic
glm_adoption.model.coefficients :hash

#=> {:AnimalType_Cat=>0.8376443692275163, :"Breed_Pit Bull Mix"=>0.28200753488859803, :"Breed_German Shepherd Mix"=>1.0518504638731023, :"Breed_Chihuahua Shorthair Mix"=>1.1960242033878856, :"Breed_Labrador Retriever Mix"=>0.445803000000512, :"Breed_Domestic Longhair Mix"=>1.898703165797653, :"Breed_Siamese Mix"=>1.5248210169271197, :"Breed_Domestic Medium Hair Mix"=>-0.19504965010288533, :Breed_other=>0.7895601504638325, :"Color_Blue/White"=>0.3748263925801828, :Color_Tan=>0.11356334165122918, :"Color_Black/Tan"=>-2.6507089126322114, :"Color_Blue Tabby"=>0.5234717706465536, :"Color_Brown Tabby"=>0.9046099720184905, :Color_White=>0.07739310267363662, :Color_Black=>0.859906249787038, :Color_Brown=>-0.003740755055106689, :"Color_Orange Tabby/White"=>0.2336674067343927, :"Color_Black/White"=>0.22564205490196415, :"Color_Brown Brindle/White"=>-0.6744314269278774, :"Color_Orange Tabby"=>2.063785952843677, :"Color_Chocolate/White"=>0.6417921901449108, :Color_Blue=>-2.1969040091451704, :Color_Calico=>-0.08386525532631824, :"Color_Brown/Black"=>0.35936722899161305, :Color_Tricolor=>-0.11440457799048752, :"Color_White/Black"=>-2.3593561796090383, :Color_Tortie=>-0.4325130799770577, :"Color_Tan/White"=>0.09637439333330515, :"Color_Brown Tabby/White"=>0.12304448360566177, :"Color_White/Brown"=>0.5867441296328475, :Color_other=>0.08821407092892847, :"SexuponOutcome_Spayed Female"=>0.32626712478395975, :"SexuponOutcome_Intact Male"=>-3.971505056680895, :"SexuponOutcome_Intact Female"=>-3.619095491410668, :SexuponOutcome_Unknown=>-102.73807712615843, :"AgeuponOutcome(Weeks)"=>-0.006959545305620043}

```

Also through the work of [Alexej Gossmann](https://github.com/agisga), one can also perdiction on new data using the model.

```ruby
predict = glm_adoption.predict test
predict.map! { |i| i < 0.5 ? 0 : 1 }
predict.head 5
```
![](http://i67.tinypic.com/r1af7p.png)

This I believe makes Statsample-GLM very convenient to use.

See [this](http://nbviewer.jupyter.org/github/SciRuby/sciruby-notebooks/blob/master/Data%20Analysis/Categorical%20Data/examples/%5BExample%5D%20Formula%20language%20in%20Statsample-GLM.ipynb) for a complete example.

#### Other

In addition to above mentioned changed there are some other considerable changes:
- Improving overall structure of indexing in Daru and adding more capabilities. See [this](http://nbviewer.jupyter.org/github/SciRuby/sciruby-notebooks/blob/master/Data%20Analysis/Categorical%20Data/Indexing%20in%20Vector.ipynb) and [this](http://nbviewer.jupyter.org/github/SciRuby/sciruby-notebooks/blob/master/Data%20Analysis/Categorical%20Data/Indexing%20in%20DataFrame.ipynb).
- `CategoricalIndex` to handle the case when index column is a categorical data. More about it [here](http://lokeshh.github.io/blog/2016/06/14/categorical-index/).
- Improving missing value API in Daru. Read more about it [here](http://lokeshh.github.io/blog/2016/08/18/improve-missing-values-api-in-daru/).
- Configuring guard to enable automatic testing. More info [here](https://github.com/v0dro/daru/blob/master/CONTRIBUTING.md#testing).


#### Documentation

You can read about all my work in detail [here](http://lokeshh.github.io/blog/2016/06/21/categorical-data/).

I hope with these additions one will be able to see data more clearly with Daru :)

