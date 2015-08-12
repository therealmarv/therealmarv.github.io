---
layout: post
title: "Comparing MathML support in HTML5 between Chrome 24, Firefox 18 and Opera 12"
date: 2013-01-11
comments: true
categories:
 - mathjax
 - firefox
 - mathml
 - html5
 - chrome
 - opera
---

Yesterday Google was releasing Google Chrome 24 stable which should have
native MathML support. I waited for this release for a long time because
Chrome is my standard browser and I know Firefox does a very good job
with MathML since Firefox 3.

So I tested Chrome 24 with this MathML browser test
site: https://eyeasme.com/Joe/MathML/MathML_browser_test

First I was very excited because the first examples looked promising and
right in Chrome. But when looking at more complex MathML Chrome rendered
it wrong.

I installed Chromium Dev 26.0.1380.0 and I was hoping the Chromium team
already fixed it. But my hope did not lasted long. Chromium 26 was
showing me the same results as Chrome 24. This [bug is already reported to Chromium's bug tracker](http://code.google.com/p/chromium/issues/detail?id=169413)
(star it if you're interested in a fix).

I recently installed also Opera just to cover the last to me known
modern browser which supports MathML natively (winkwink Internet
Explorer). Opera had the most issues with rendering and some serious
font problems with MathML on my Ubuntu machine.

So I guess we still need to use the very
good [MathJax](http://www.mathjax.org/) JS library for a longer time to
display all kind of MathML right in every browser except Firefox.

Result:

* Firefox: Very good, near perfect native MathML support.
* Chrome: Partial MathML support.
* Opera: Partial MathML support but too many rendering issues.
* Internet Explorer: No MathML support!

But see the rendered browser results yourself:

[![MathML results](http://i.imgur.com/Z8n59.png)](http://i.imgur.com/Z8n59.png)
