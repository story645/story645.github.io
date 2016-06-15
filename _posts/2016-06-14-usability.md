---
layout: post
title:  "This is really a usability project"
date:   2016-06-14 14:20:41 -0400
excerpt_separator: <!--more-->
categories: jekyll update
---

So, this week I took a semi-break<sup>1</sup> from GSOC 'cause my fellowship ran a [weeklong series of workshops](https://gcdigitalfellows.github.io/) that were aimed at getting academics in humanaties and the softer side of STEM up to speed with what they call "digital tools" and what the rest of us call using code & software to do things (or sometimes scientific programming). So amongst others, I taught some English majors, librarians, biologists, and a lot of Business majors [how to use pandas and sklearn](https://github.com/GCDigitalFellows/gcdri_ts_cat_ml). And that involved showing them code that looks like this:

```python

comp1 = pca_weights.T[0]

fig = plt.figure()
ax1 = fig.add_subplot(2,1,1)
ax1.bar(range(comp1.shape[0]), comp1, align='center')
ax1.set_xticks(range(comp1.shape[0]))
ax1.set_xticklabels(dfFV.keys(), rotation=90)
ax1.set_xlim(-.5,12.5)
ax1.set_ylim(-.8,.8)
ax1.set_ylabel("X axis")
```
<!--more-->
For those of us who code a ton this makes some sense: tell where the bars go, and then how high. It's just an extension of x coordinates and y coordinates. Now imagine you haven't seen a cartesian plane (x/y graph) in years, possibly decades, and you possibly haven't made a chart in about that long. And coding is kinda new and so you may not know that .shape is a thing, much less what it does. So even totally ignoring the prettying boilerplate, currently teaching someone how to use matplotlib to make a simple bar plot requires:

```python
ax1.bar(range(comp1.shape[0]), comp1, align='center')
ax1.set_xticks(range(comp1.shape[0]))
ax1.set_xticklabels(dfFV.keys(), rotation=90)
```

which requires the following conceptual explainations 'cause I've tried handwaving and people just really hate having everything handwaved away:

```python
ax1.bar(range(comp1.shape[0]), comp1, align='center')
```

. notation & objects & functions - fine, this is standard anyway & why you need to pass in the x (which nobody cares about for bars) and y (which is intuitive). And yes you can leave it at that, but the plot is close to useless at this stage, so next:

```python
ax1.set_xticks(range(comp1.shape[0]))
ax1.set_xticklabels(dfFV.keys(), rotation=90)
```
Explain ticks and labels and why they need to be seperated to an audience that mostly thinks excel is complicated. And kwargs, if you haven't yet. And go slowly 'cause your audience hasn't quite grokked functions yet. And remember that this was probably a throwaway in the grand scheme of whatever you really wanted to teach...

And most importantly none of this is intuitive 'cause bars don't really have an x component, at least not the way we typically expect them to. They just show catagorical data, or heights...the x is typically hidden and intuited by the plotting routine. Pandas understands that sometimes it's better to just leave things magic via their plotting libraries, and so we aim to extend that to matplotlib natively so that a bit of a maintenance load gets lifted. And so other plotting libraries like xarray can follow suite. 


<sup>1</sup>I also forked pandas and matplotlib and installed those on my machines. The latter has been rather cranky 'cause of a freetype issue, so my mentor suggested the following before build: `export MPLLOCALFREETYPE=1`
