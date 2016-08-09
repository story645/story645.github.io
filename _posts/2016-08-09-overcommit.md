---
layout: post
title:  "Overcommitting is a terrible flaw"
date:   2016-08-09 16:21:00 -0400
excerpt_separator: <!--more-->
categories: jekyll update
---
During my last weekly meeting with one of my mentors, I admitted that I was a bad GSOCer 'cause I was terribly overcommited. 
And 'tis true...missed out last weeks post 'cause was flailing under todos for my grad school career and 'cause I've been 
mentoring high schoolers all summer and their end of year poster was due Friday. I also taught 20 of them for a week and 
am now buried under a mound of grading. This is about the moment when I'm quite thankful that my advisors are quasi-academics as they understand this well. But I did get [something done](https://github.com/matplotlib/matplotlib/pull/6889) last week:

![img](/assets/figs/multiax.png)
<!--more-->

which is the very common use case of:

```python
fig, ax = plt.subplots()
ax.plot(['a', 'c', 'e'], label="plot 1")
ax.plot(['a', 'b', 'd'], label="plot 2")
ax.plot(['b', 'e', 'd'], label="plot3")
ax.legend()
```

This PR is held up by my premature adoption of Pytest; while matplotlib is moving towards it, the codebase doesn't
yet support it. My other PR is stuck in [review](https://github.com/matplotlib/matplotlib/pull/6889). I've also moved [my todo 
list](https://github.com/story645/matplotlib/issues) to the issue tracker on my fork of matplotlib. 
