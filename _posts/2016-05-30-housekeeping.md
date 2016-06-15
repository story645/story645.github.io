---
layout: post
title:  "Orientation by way of housekeeping"
date:   2016-06-14 14:20:41 -0400
excerpt_separator: <!--more-->
categories: jekyll update
---

My project, catagorical axis, intersects with all sorts of pieces of the codebase and I had no idea where to start. So, since 
I was feeling kind of lost and overwhelmed in the sea that is the [matplotlib codebase](https://github.com/matplotlib/matplotlib), 
my mentors suggested that I do an ad-hoc literature review of the issues and pull requests to see which ones are related to my project.
Since I wanted to keep track of what I'd read, I wrote a simple script to drop the issues into a csv:
<!--more-->

```python
import requests
import pandas as pd

issue_list = []
for page in range(1,30):
    r = requests.get('https://api.github.com/repos/matplotlib/matplotlib/issues', 
                    params={'page':page})
                    
    for issue in r.json():
        row = {k:issue[k] for k in ('number', 'title')}
        row.update({k:issue[k][:10] for k in ('created_at', 'updated_at')})
        row['labels'] = [lab['name'] for lab in issue['labels']]
        row['pull_request'] = ('pull_request' in issue)
        issue_list.append(row)

columns=['number', 'title', 'created_at', 'updated_at', 'labels', 'pull_request']
df = pd.DataFrame(issue_list, columns=columns)
df.to_csv("github", encoding='utf-8')
```

You can read my notes (and contribute) here: [
Matplotlib Issues & PRs](https://docs.google.com/spreadsheets/d/1PkxqqVAzrWuGtQ1KJJ-RrrnA17wMRniy1W4gVPOBSK0/edit?usp=sharing) Since I was already tackling the issue tracker, I also started flagging stalled issues 'cause during the application period I had gotten very frustrated trying to figure out which issues I could actually tackle and which were mired in discussion. My mentors just said I could create a stalled label, so I plan to have some fun with that.

Going down this rabbit hole, I was also given contributor rights on the matplotlib repo so that I could create labels and label issues. It's been kinda therapeutic, but more importantly has given me a crash course of sorts on how the matplotlib community operates. I'm starting to see how issues get reported, and resolved, and why they get stalled and I get to feel the joy of finding the stray issue that's been resolved. I've even found a couple of issues that I could probably tackle. </p>

If you want to get involved in a project and are finding it hard to figure out where to start, I urge you to get out your metaphorical duster and join in on some housekeeping. And drop into the [gitter](https://gitter.im/matplotlib/matplotlib) when you have questions. 
