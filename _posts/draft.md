
![workflow](assets/figs/Screen Shot 2016-07-08 at 5.28.03 PM.png)
Let's talk workflow 'cause I've spent a rather insane amount of time fighting with git. And I'm not new to git, 
having been using it for a few years. I'm much more a fan of mercurial, 


Oh, let me tell you about the many ways I hate git. I'm not new to git, and I use mercurial heavily and once upon a time 
I've even used SVN (which I think makes me an old), but I've never really used version control in a large project, 
multiple branches & developers setting. And my go to is commit small and often...so my commit branch tends to be super muddled...
So rebase, right? Uh...yeah...it's confusing as all out and I had to try it a zillion times before I found 
[a tutorial that was clear to me](http://gitready.com/advanced/2009/02/10/squashing-commits-with-rebase.html)
and grokked the ever so unintuitive "PICK THE OLDEST COMMIT" and squash everything else. I still haven't found anything that 
clearly explains got to fix merge conflicts, which wasn't an issue in matplotlib but in a way old open PR on nltk. It took me 
multiple branches and resets and rebases before I got a somewhat clean pull request that was up to date with develop. I should 
totally rebase to clean up the commit stream, but that's a rabbit hole I'm avoiding unless the nltk devs request it. My mentor 
also helpfully pointed me to the [dev workflow page](http://matplotlib.org/devel/gitwash/development_workflow.html), so that's 
bookmarked for future reference. 

