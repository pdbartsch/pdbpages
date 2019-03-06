---
title: "Common Field Calculations in ArcGIS"
date: 2019-03-06T08:21:04-08:00
publishdate: 2019-03-06
lastmod: 2019-03-06
draft: false
tags: ['ArcGIS-Desktop', 'Python']
---

The ArcGIS field calculator can save a person time and allow for fairly quick data cleanup IF you can remember how to use it.  I don't know about you, but for me the layout of the field calculator, the separation of the formula field and the code block area and the minor changes from standard Python formatting are enough to add a few minutes to each task.  That's why I started making notes for even the simplest of tasks.  I go back to them often saving myself time and frustrastion.  

Maybe someday I'll memorize these, but for now this method works for me.

Below is a list of some simple field calculations that I constantly go back to for use as a starting place.  I'll add to these as I think of them.  Maybe you'll find one useful.




{{< figure src="/img/fieldcalc.png" title="ArcGIS Pro Field Calculator" >}}


### Common Field Calculations using Python:

- **Chain commands together.**  In this example I start with a field called *TextField* with some messy data.  Suppose I have Oak Trees listed as *oak, Oak, Oak tree, and oak tree* but I want to normalize them all to read simply *Oak Tree*.  I can chain a few simple commands together to accomplish this:
{{< highlight python >}}

!TextField!.lower().replace("tree", " ").replace("oak", "Oak Tree")

{{< /highlight >}}

- **Using the Code Block.**  The Code Block portion of the Calculate Field tool in ArcGIS allows for the use of any Python function, you can even import modules and define your own functions.  The format is not that complicated once you've done a few.  I think of the Code Block area as a place to define functions which I then call in the smaller Expression box.  In the following example I want to create an abbreviated code for each tree species by taking the first two letters of the genus and concatenating them together with the first two letters of the species.  I can accomplish that quickly like this:
{{< highlight python >}}
# expression:
abbreviate(!SciName!)

# code block:
def abbreviate(x):
    words = x.split()
    letters = [word[:2] for word in words]
    return "".join(letters)

{{< /highlight >}}

- **Math.** To convert from square footage to acres and round to two decimal places you could do something like this:
{{< highlight python >}}

# expression:
sqft_to_acres(!Shape_Area!)

# code block:
def sqft_to_acres(area):
    return round((area / 43560),2)
{{< /highlight >}}
You could accomplish the same result with a single line as below, but I personally prefer to use the Code Block to make things more legible and repeatable:
{{< highlight python >}}
round((!Shape_Area! / 43560),2)
{{< /highlight >}}