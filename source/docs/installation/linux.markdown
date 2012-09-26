---
layout: page
title: "*NIX Install Instructions"
comments: false
sharing: true
footer: true
---

These packages are needed for the SciRuby gem, and these commands should work for Debian and Ubuntu flavors. If you're
using something else, please <a href="mailto:sciruby-dev@googlegroups.com">email sciruby-dev</a> with the steps you took.

<pre><code>apt-get update # update package sources
apt-get install libgtk2.0-dev librsvg2-dev libcairo2-dev
apt-get install libgtksourceview2.0-dev # optional, for code editor
</code></pre>

If you have problems, you might consult [the Green Shoes wiki](https://github.com/ashbb/green_shoes/wiki/Building-Green-Shoes-on-Ubuntu).