---
layout: post
title:  "Code Calling Code Calling Code"
date:   2016-06-20 12:31:00 -0400
excerpt_separator: <!--more-->
categories: jekyll update
---
This week I got knee deep in code, kinda literally. Matplotlib protocol is to wrap proposed changes in a pull request
so that conversation is grounded in one place, so you can read all about my confusion at 
[PR #6612](https://github.com/matplotlib/matplotlib/pull/6612). You'll also see that I have a tendency to treat my code like 
running notes...

But let me work back to that. The starting point is that I'm writing what's called a convertor-in matplotlib land this is 
the underlying mechanism that takes data that's not numerical and plots it. I've been looking at a lot of Pandas [date convertors](https://github.com/pydata/pandas/blob/master/pandas/tseries/converter.py)
& [tests](https://github.com/pydata/pandas/blob/master/pandas/tseries/tests/test_converter.py) and 
[mpl/dateconvertor](https://github.com/matplotlib/matplotlib/blob/master/lib/matplotlib/dates.py#L1524), and 
[mpl/jpl_units](https://github.com/matplotlib/matplotlib/tree/master/lib/matplotlib/testing/jpl_units) 
and [mpl/test_units](https://github.com/matplotlib/matplotlib/blob/master/lib/matplotlib/tests/test_units.py) 
and like the title of this post implies, I've been going up and down the matplotlib call stack trying to sort out when the 
conversion functions get called. And spending a lot of time on [gitter/mpl](https://gitter.im/matplotlib/matplotlib)

<!--more-->

It's also a little simplistic to just say conversion functions, 'cause far as I can figure out, 
locators and formatters kick in too. So going by the official [maplotlib units docs](http://matplotlib.org/api/units_api.html), this
is the skeleton of what I'm working with. There's some actual code too, but the skeleton is more important from a holistic 
how does this fit perspective? And I reneged on starting with pandas categorical data when I started attempting trying 
to tease out the datetype for registration. So I'm writing a convertor for strings.  

The register function is arguably the most important piece of this as it's the one that says 
"hey, see this type, use this convertor":

```python
def register():
  if six.PY3:        
    units.registry[str] = CategoricalConverter()
  elif six.PY2:
    units.registry[basestring] = CategoricalConverter()
```
This is also my little shout out to [six](https://pythonhosted.org/six/), which is a python 2/3 compatability library for 
when your code has to work with both. And since my code deals with strings, I'm running into `str vs. basestring` a lot.

The actual conversion machinary is a subclass of units.ConversionInterface. 

```python
class CategoricalConverter(units.ConversionInterface):
  @staticmethod 
  def convert(value, unit, axis):
    vals = np.asarray(value, dtype='str')
    #not terribly interesting code here 
    #to encode vals (list of strings) into list of numbers
    return vals.astype('int')
  
  @staticmethod
  def axisinfo(unit, axis):
    majloc = CategoricalFormatter()
    majfmt = CategoricalLocator()
    return units.AxisInfo(majloc=majloc, majfmt=majfmt, label=None)
    
  @staticmethod
  def default_units(x,axis):  
    """Maybe should support dict keys/fieldnames as units?"""
    return None
```

`CategoricalFormatter` and `CategoricalLocator` are just stubs that inherit `FixedLocator` & `FixedFormatter` 'cause I 
figure that for catagorical data, it's important to have a tick and label for each unique category, which is the 
[fixed tickers](http://matplotlib.org/api/ticker_api.html) job. Locator handles ticks & formatter handles labeling, and 
I'm subclassing them so that potentially later more advanced categorical groupings can be slotted in. 

My mentor had to explain to me what a `@staticmethod` was. For anybody like me, it means that convert can be called without making a CategoricalConverter object:

```python
a = CategoricalConverter.convert(['a','b','c'])
```
which essentially allows the `CategoricalConverter` class to act as an ersatz namespace, which is how many things are 
structured in the matplotlib codebase. Granted, this realization also triggered a reminder of how `cls` methods worked, which 
was kinda important as I'm a huge fan of TDD (test driven development) and so I have all sorts of tests, but I'm gonna focus 
on the one with the most moving pieces. I'm using [mock](https://docs.python.org/dev/library/unittest.mock.html) because it 
allows for inspection of it's elements. 

This test isn't a test of whether the functions work, it's a test of "if registered correctly, does plot actually 
use these functions"-which is why I should rewrite convert so that it doesn't call the real catagorical 
convert function and instead just returns what's expected for x and y. 

```python
class TestPlot(unittest.TestCase):
  """Use mock to check that plot calls the conversion interface"""
  @classmethod
  def setupClass(cls):
    cls.cc = munits.ConversionInterface()
      
    def convert(value, unit, axis):
      return cat.CategoricalConverter.convert(value, unit, axis)
       
    cls.cc.convert = MagicMock(side_effect=convert)
    #snip out rest for the moment
    
  def setUp(self):
    self.x = ["a","b","c","a"]
    self.y = ["d","d", "e","g"]
      
  def test_plot(self):
    fig, ax = plt.subplots(1,1)
    l, = plt.plot(self.x, self.y)
    self.assertTrue(TestPlot.cc.convert.called)
```

While going down the TDD rabbit hole, I worked my way into a weird circle of what am I supposed to be testing? but 
it also helped me scope down this project to just strings (at least on a first pass). And I learned that nose is deprecated, 
so at the moment I'm defaulting to plain old [unittest](https://docs.python.org/3.5/library/unittest.html?highlight=unittest) 
with a caveat that I may change the library as the code develops and my mentors dictate. 

