---
layout: post
title:  "It works! It works"
date:   2016-05-03 14:20:41 -0400
excerpt_separator: <!--more-->
categories: jekyll update
---

Well something works, more particularly you can now plot strings...What this means is that thing 
I mentioned in the very bottom of my [very first post](2016-05-03-better_axis.md), 
that we could hopefully simplify bar enough that users can quit with the `range(labels)` madness 
is now a thing that is possible. So this:

![bar2](/assets/figs/bar2.png)

can now be created using this:
```
data = {'apples':10, 'oranges':15, 'lemons':5, 'limes':20}
fig = plt.figure()`
ax = fig.add_subplot(1,1,1)`
ax.bar(data.keys(), data.values(), align='center', color='lightgray')
ax.xaxis.set_tick_params(size=0)
```
which makes me so very happy 'cause one less level of abstraction always rocks. And yes, 
having to create arbitrary indexes for labels is an abstraction.

Note: Python 3+ dicts [are special](https://docs.python.org/3/library/stdtypes.html#dictionary-view-objects) and 
so you need to do this to make it work:
```python
ax.bar(list(data.keys()), list(data.values()), align='center', color='lightgray')
```
And yes, a new item on my todo list is now: add builtin support for Python 3 dicts into matplotlib 'cause the thought 
of having to explain views to newbies (or frankly most people) makes me shudder. The official Python 3 documentation 
is terrible for this 'cause it commits the cardinal sin of explanation by using the term it's trying to 
explain in the explaination:
```
They provide a dynamic view on the dictionary’s entries, which means that when the dictionary changes, 
the view reflects these changes.
```

