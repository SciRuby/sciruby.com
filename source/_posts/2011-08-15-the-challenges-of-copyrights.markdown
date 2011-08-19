---
layout: post
title: "The Challenges of Copyrights"
date: 2011-08-15 19:56
comments: true
author: John Woods
categories: [Algorithms,Licenses,Meta]
---
Last week, I thought it would be fun to translate some of the _[Numerical Recipes](http://nr.com/)_ algorithms into Ruby,
 specifically those for the Gamma distribution and incomplete gamma function. I got them all working pretty well, and
 they were only a hundred or so lines of code altogether.

But a voice in the back of my head said, "Hey, you should ask Bill Press if he wants to be involved."

[Bill Press](http://en.wikipedia.org/wiki/William_H._Press), by the way, is a pretty cool astrophysicist who decided he
 wanted to come work here at UT as a computational biologist instead. He also happens to be first author on _Numerical
 Recipes_, and a fairly regular attendee at my lab's weekly meetings. Once he had to pause my exam to take a call
 from the White House, which was also pretty awesome.

So I emailed Dr. Press to ask him if he'd be interested in helping out with SciRuby, and specifically if he might be
 willing to donate a copy of NR. He was happy to donate a copy and to help out in whatever way he could, he wrote back,
 but the license for NR wouldn't allow us to use the code from the book.

My stomach sank. I had missed that disclaimer, and I was used to dealing with more-or-less public domain (or at least
 GPLed) Ruby code.

The email continued. We should be careful using other libraries, because many of them were illegally derived from
 copyrighted materials.

Several interesting discussions ensued ([including this public one](https://plus.google.com/108379995018215027591/posts/2AefG8i3nUX)).
 The take-away is that many people don't understand code copyrights (I among them). Consequently, most governments don't
 understand copyrights (and it doesn't help that U.S. Congress isn't exactly full of programmers).

Some were critical of Press and his coauthors for not simply giving us special permission to use his code, but such a
 gimme would be complicated for NR. If they permitted us to use NR code in SciRuby, that code would then be available to
 anyone else to use under the terms of the GPL. I imagine his publisher would be less than thrilled.

So here are some general guidelines for those who want to contribute code to an open-source scientific project,
 particularly SciRuby. Please keep in mind that I am not a lawyer, and take any of this with a grain of salt.

 * If in doubt, check with the author.

 * Do unto others as you would have them do unto you. In other words, honor closed source licenses if you wish your open
   source license to be honored.

 * *Always cite your sources.* If a copyright owner discovers apparently derivative code without a citation, it's easy
   to him or her to assume you plagiarized it directly from his or her work. If it has a citation, someone else gets the
   blame, or the copyright owner can determine that the code is only _apparently_, but not _actually_, derivative.

 * If you write your own code, derived from your own math, document it. Scan your derivation and make it public (send it
   to our Google Group, for example).

Any more words of wisdom? Stick them in the comments.