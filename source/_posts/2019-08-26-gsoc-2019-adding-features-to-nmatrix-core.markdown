---
layout: post
title: "GSoC 2019: Adding features to NMatrix core"
date: 2019-08-26 23:49
author: Udit Gulati
comments: true
categories: [GSoC, GSoC 2019, Ruby, NMatrix, NumRuby]
---

The GSoC coding period has come to an end. It's been quite a learning experience for me and has given me an opportunity to come out of my comfort zone and explore new things. I feel quite delighted knowing that I have contributed a significant amount to the project. This post summarizes my work on [NumRuby](https://github.com/sciruby/numruby) during the GSoC period.

GSoC 2019 proposal - [https://docs.google.com/document/d/1MR01QZeX_8h7a16nmkOYlyrVt--osB1Yg9Vo0xXYtSw/edit?usp=sharing](https://docs.google.com/document/d/1MR01QZeX_8h7a16nmkOYlyrVt--osB1Yg9Vo0xXYtSw/edit?usp=sharing)

## Pull requests

+   Indexing, iterating, slicing - [https://github.com/SciRuby/numruby/pull/19](https://github.com/SciRuby/numruby/pull/19)
+   Fix compiler warnings - [https://github.com/SciRuby/numruby/pull/21](https://github.com/SciRuby/numruby/pull/21)
+   Dimensional iterators - [https://github.com/SciRuby/numruby/pull/22](https://github.com/SciRuby/numruby/pull/22)
+   Broadcasting - [https://github.com/SciRuby/numruby/pull/23](https://github.com/SciRuby/numruby/pull/23)
+   Lapack wrappers - [https://github.com/SciRuby/numruby/pull/26](https://github.com/SciRuby/numruby/pull/26)
+   NumRuby::Linalg - [https://github.com/SciRuby/numruby/pull/30](https://github.com/SciRuby/numruby/pull/30)

## Blog Posts

+   GSoC project introduction - [https://uditgulati.github.io/blog/open-source/2019/05/21/gsoc-project-introduction-nmatrix.html](https://uditgulati.github.io/blog/open-source/2019/05/21/gsoc-project-introduction-nmatrix.html)
+   GSoC weeks 1, 2 - [https://uditgulati.github.io/blog/open-source/2019/06/10/gsoc-week-1-2.html](https://uditgulati.github.io/blog/open-source/2019/06/10/gsoc-week-1-2.html)
+   GSoC weeks 3, 4 - [https://uditgulati.github.io/blog/open-source/2019/06/26/gsoc-week-3-4.html](https://uditgulati.github.io/blog/open-source/2019/06/26/gsoc-week-3-4.html)
+   GSoC weeks 5, 6 - [https://uditgulati.github.io/blog/open-source/2019/07/10/gsoc-week-5-6.html](https://uditgulati.github.io/blog/open-source/2019/07/10/gsoc-week-5-6.html)
+   GSoC weeks 7, 8 - [https://uditgulati.github.io/blog/open-source/2019/07/25/gsoc-week-7-8.html](https://uditgulati.github.io/blog/open-source/2019/07/25/gsoc-week-7-8.html)
+   GSoC weeks 9, 10 - [https://uditgulati.github.io/blog/open-source/2019/08/16/gsoc-week-9-10.html](https://uditgulati.github.io/blog/open-source/2019/08/16/gsoc-week-9-10.html)

## Future work

+   As these features are core to NMatrix, they need to be tested more tightly and benchmarked. Also, since this is the first implementation of the features, some modifications can make these features more reliable and faster.
+   More iterators are to be implemented in order to have a richer NMatrix API.
+   The issue with slicing needs to be fixed soon. Also, need to standardize the formatting used for slice range specification.
+   Broadcasting needs to be tested and benchmarked. This will let us improve it's reliability and performance. A few APIs could be exposed which would let a user manually broadcast a matrix to a given shape.
+   NumRuby::Lapack needs to have proper exception handling for LAPACK routines. This will be done by reading the info value returned by routine and raising exception accordingly.
+   NumRuby::Linalg can be extended to have even more methods.

## Acknowledgements

I wanna thank Ruby Science Foundation, all mentors and org admins for providing me this wonderful opportunity to enhance my knowledge and work on a project. I would also like to thank Google for organizing such a wonderful program due to which I got introduced to Open-source and Ruby Science Foundation. I especially want to thank my mentor Prasun Anand for guiding me through this period, keeping me motivated and for tolerating my procrastination.
