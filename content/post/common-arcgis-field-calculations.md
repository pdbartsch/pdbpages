---
title: "Common Field Calculations in ArcGIS"
date: 2019-03-06T08:21:04-08:00
publishdate: 2019-03-06
lastmod: 2019-03-06
draft: false
tags: ['ArcGIS-Desktop', 'Python']
---

The ArcGIS field calculator can save a person time and allow for fairly quick data cleanup IF you can remember how to use it.  I don't know about you, but for me the layout of the field calculator, the separation of the formula field and the code block area and the minor changes from standard Python formatting are enough to send me straight to the Google search bar if I haven't used it in a while.  That's why I started making notes for even the simplest of tasks.  I go back to them often saving myself time and frustrastion.  

Maybe someday I'll memorize these, but for now this works.

Below you'll find a list of some simple field calculations that I use often as a starting place.  I'll add to these as I think of them.  Maybe you'll find one useful.

{{< figure src="/img/fieldcalc.png" title="ArcGIS Pro Field Calculator" >}}

### Common Field Calculations using Python:

- **Change text to title case.** - So simple and used frequently.
{{< highlight python >}}
# expression:
!note1!.title()
{{< /highlight >}}

- **Replace one character for another.** - Probably the most common reason I use the field calculator.
{{< highlight python >}}
# expression:
!noteField!.replace("-", "_")
{{< /highlight >}}

- **Concatenate fields together.** - Very common when geocoding addresses.  
{{< highlight python >}}
# expression:
!Address! + " " + !Street! + ", " + !City! + ", " + !State! + " " + !Zip!
{{< /highlight >}}


- **Simple mathematic calculations across multiple fields.** - You can use the field calculator to extract summary statistics from a list of fields.
{{< highlight python >}}
# find the minimum value across a list of fields:
# expression:
min([!field1!, !field2!, !field3!])

# find the maximum value across a list of fields:
# expression:
max([!field1!, !field2!, !field3!])

# find the sum of all values from a list of fields:
# expression:
sum([!field1!, !field2!, !field3!])

# find the average value from a list of fields:
# expression:
sum([!field1!, !field2!, !field3!]) / len([!field1!, !field2!, !field3!])

{{< /highlight >}}


- **Chain commands together.**  In this example I start with a field called *TextField* with some messy data.  Suppose I have Oak Trees listed as *oak, Oak, Oak tree, and oak tree* but I want to normalize them all to read simply *Oak Tree*.  A few simple commands can be chained together to accomplish this.
{{< highlight python >}}
# expression:
!TextField!.lower().replace("tree", " ").replace("oak", "Oak Tree")

{{< /highlight >}}

- **Using the Code Block.**  The Code Block portion of the Calculate Field tool in ArcGIS allows for the use of any Python function, you can even import modules and define your own functions.  The format is not that complicated once you've used it a few times.  Think of the Code Block area as a place to define functions which you can later call from the Expression box.  In the following example I want to create an abbreviated code for each tree species by taking the first two letters of the genus and concatenating them together with the first two letters of the species.  I can accomplish that quickly like this:
{{< highlight python >}}
# expression:
abbreviate(!SciName!)

# code block:
def abbreviate(x):
    words = x.split()
    letters = [word[:2] for word in words]
    return "".join(letters)

{{< /highlight >}}

- **Unit conversion using math.** To convert from square footage to acres and round to two decimal places you could do something like this:
{{< highlight python >}}

# expression:
sqft_to_acres(!Shape_Area!)

# code block:
def sqft_to_acres(area):
    return round((area / 43560),2)
{{< /highlight >}}
You could accomplish the same result with a single line as below, but I personally prefer to use the Code Block to make things more repeatable and to allow for more complex functions in the future:
{{< highlight python >}}
round((!Shape_Area! / 43560),2)
{{< /highlight >}}

- **Another way to convert units.** - As detailed [here](https://pro.arcgis.com/en/pro-app/tool-reference/data-management/calculate-field-examples.htm#ESRI_SECTION1_2C1A27476FD54D949723FA8DFC9306B2), it's possible to use the ArcGIS Pro built in geometry unit conversions rather than doing your own calculations.  Have a look at the esri link above to see all of the built in unit of measurement conversions.  Note that the format of field names is slightly different when referring to geometry columns as compared to other fields:
{{< highlight python >}}
# Current area to hectares
# expression:
!shape.area@hectares!

# Current length to kilometers
# expression:
!shape.length@KILOMETERS!
{{< /highlight >}}
