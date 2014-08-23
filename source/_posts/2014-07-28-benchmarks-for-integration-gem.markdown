---
layout: post
title: "Benchmarks for Integration Gem"
date: 2014-07-28 02:44
author: Rajat Kapoor
comments: true
categories: [GSOC2014,GSOC,Integration,Students]
---

The Integration gem now includes quite a few functions from which to
choose. To make this choice easier, I have done some benchmarking to
see how fast and acuurate each of these functions are.  The results of
these tests for some randomly chosen functions are given below. The
speed test reports shows the user CPU time, system CPU time, the sum
of the user and system CPU times, and the elapsed real time to perform
100 iterations of the code. The unit of time is seconds.

## For function f(x)= x^2+3*x+7 on interval (0,1)

### Speed Tests

```
Benchmarking with 100 iterations
                                user     system      total        real
rectangle                  72.040000   0.010000  72.050000 ( 72.058014)
                                user     system      total        real
trapezoid                 105.500000   0.000000 105.500000 (105.489225)
                                user     system      total        real
simpson                     0.020000   0.000000   0.020000 (  0.017601)
                                user     system      total        real
romberg                     0.000000   0.000000   0.000000 (  0.002219)
                                user     system      total        real
adaptive_quadrature         0.010000   0.000000   0.010000 (  0.002757)
                                user     system      total        real
gauss                       0.010000   0.000000   0.010000 (  0.006439)
                                user     system      total        real
gauss_kronrod               0.000000   0.000000   0.000000 (  0.008615)
                                user     system      total        real
simpson3by8                 0.080000   0.000000   0.080000 (  0.074791)
                                user     system      total        real
boole                       0.100000   0.000000   0.100000 (  0.100300)
                                user     system      total        real
open_trapezoid            181.470000   0.050000 181.520000 (181.572745)
                                user     system      total        real
milne                       0.060000   0.000000   0.060000 (  0.058677)
                                user     system      total        real
qng                         0.000000   0.000000   0.000000 (  0.003788)
                                user     system      total        real
qag                         0.010000   0.000000   0.010000 (  0.010407)
```

### Accuracy Tests
```
+---------------------+-------------------+-------------------+------------------------+-------------------+
|       Method        |      Result       |   Actual Result   |         Error          |     Accuracy      |
+---------------------+-------------------+-------------------+------------------------+-------------------+
| rectangle           | 8.833333324123249 | 8.833333333333334 | 9.210085138988688e-09  | 99.99999989573489 |
| trapezoid           | 8.83333334502252  | 8.833333333333334 | 1.1689186507624072e-08 | 99.99999986766959 |
| simpson             | 8.833333333333332 | 8.833333333333334 | 1.7763568394002505e-15 | 99.99999999999997 |
| romberg             | 9.0               | 8.833333333333334 | 0.16666666666666607    | 98.11320754716982 |
| adaptive_quadrature | 8.833333333333334 | 8.833333333333334 | 0.0                    | 100.0             |
| gauss               | 8.750000000006125 | 8.833333333333334 | 0.08333333332720905    | 99.05660377365425 |
| gauss_kronrod       | 8.75              | 8.833333333333334 | 0.08333333333333393    | 99.0566037735849  |
| simpson3by8         | 8.833333333333336 | 8.833333333333334 | 1.7763568394002505e-15 | 99.99999999999997 |
| boole               | 8.833333333333336 | 8.833333333333334 | 1.7763568394002505e-15 | 99.99999999999997 |
| open_trapezoid      | 8.833333325264693 | 8.833333333333334 | 8.068640866554233e-09  | 99.9999999086569  |
| milne               | 8.833333333333334 | 8.833333333333334 | 0.0                    | 100.0             |
| qng                 | 8.833333333333334 | 8.833333333333334 | 0.0                    | 100.0             |
| qag                 | 8.833333333333336 | 8.833333333333334 | 1.7763568394002505e-15 | 99.99999999999997 |
+---------------------+-------------------+-------------------+------------------------+-------------------+
```
## For function f(x) = x^5+7*x^2+2*cos(x) on interval (0,1)

### Speed Tests
```
Benchmarking with 100 iterations
                                user     system      total        real
rectangle                 534.790000   0.510000 535.300000 (538.238675)
                                user     system      total        real
trapezoid                 835.950000   0.540000 836.490000 (839.233313)
                                user     system      total        real
simpson                     0.650000   0.000000   0.650000 (  0.652694)
                                user     system      total        real
romberg                     0.010000   0.000000   0.010000 (  0.002073)
                                user     system      total        real
adaptive_quadrature         0.170000   0.000000   0.170000 (  0.167520)
                                user     system      total        real
gauss                       0.000000   0.000000   0.000000 (  0.007621)
                                user     system      total        real
gauss_kronrod               0.020000   0.000000   0.020000 (  0.010610)
                                user     system      total        real
simpson3by8                 0.820000   0.000000   0.820000 (  0.831039)
                                user     system      total        real
boole                       0.120000   0.000000   0.120000 (  0.119929)
                                user     system      total        real
open_trapezoid            1019.100000   0.150000 1019.250000 (1020.034828)
                                user     system      total        real
milne                       0.980000   0.000000   0.980000 (  0.970671)
                                user     system      total        real
qng                         0.000000   0.000000   0.000000 (  0.008263)
                                user     system      total        real
qag                         0.020000   0.000000   0.020000 (  0.020050)
```

### Accuracy Tests

```
+---------------------+--------------------+--------------------+------------------------+-------------------+
|       Method        |       Result       |   Actual Result    |         Error          |     Accuracy      |
+---------------------+--------------------+--------------------+------------------------+-------------------+
| rectangle           | 4.182941950501391  | 4.1829419696157935 | 1.9114402505238104e-08 | 99.99999954303927 |
| trapezoid           | 4.182941993679488  | 4.1829419696157935 | 2.406369414842402e-08  | 99.99999942471844 |
| simpson             | 4.182941969798875  | 4.1829419696157935 | 1.8308110583120651e-10 | 99.99999999562314 |
| romberg             | 5.54030230586814   | 4.1829419696157935 | 1.3573603362523468     | 67.5501035847021  |
| adaptive_quadrature | 4.182941969702552  | 4.1829419696157935 | 8.675815621472793e-11  | 99.9999999979259  |
| gauss               | 3.5364151237832213 | 4.1829419696157935 | 0.6465268458325721     | 84.54372901826423 |
| gauss_kronrod       | 3.5364151237807455 | 4.1829419696157935 | 0.6465268458350479     | 84.54372901820506 |
| simpson3by8         | 4.182941969676288  | 4.1829419696157935 | 6.049472034419523e-11  | 99.99999999855378 |
| boole               | 4.182941969615793  | 4.1829419696157935 | 8.881784197001252e-16  | 99.99999999999997 |
| open_trapezoid      | 4.182941952971977  | 4.1829419696157935 | 1.6643816103112385e-08 | 99.99999960210263 |
| milne               | 4.18294196954598   | 4.1829419696157935 | 6.981348832368894e-11  | 99.99999999833099 |
| qng                 | 4.182941969615793  | 4.1829419696157935 | 8.881784197001252e-16  | 99.99999999999997 |
| qag                 | 4.1829419696157935 | 4.1829419696157935 | 0.0                    | 100.0             |
+---------------------+--------------------+--------------------+------------------------+-------------------+
```

If you are keen to test any function of your choice you can pull from
[my Integration gem fork](https://github.com/rajatkapoor/integration)
and change the files in the benchmark folder. For both the files,
`accuracy.rb` and `speed.rb`, change the line `func = lambda{|x| f(x)}`
with `f(x)` as your desired function. In case you want to perform
accuracy benchmarks, make sure that you know the exact result of the
integration operation before hand and assign this value to the
variable `actual_result` Feel free to comment if you face any problem
running the benchmarks with your desired functions.