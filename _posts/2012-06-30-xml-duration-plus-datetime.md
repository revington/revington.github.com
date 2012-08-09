
---
layout: post
category : programming
tags : [c#, xml]
---
{% include JB/setup %}

Have you ever tried to convert from [xml duration](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#duration) to TimeSpan and then added to a DateTime?

This is wrong if the duration expresed years or months.
This is because TimeSpan data type does not have fields for years nor months and
the lack of these fields forces the conversion to days.
So "P1Y" converted to TimeSpan is always 365 days and "P1M" is always 30 days.

## Fix

We are going to directly create a function that adds xml duration to DateTime.
{% highlight c# %}
using System;

	string duration = "P1Y";
	DateTime date = new DateTime(2011,2,28);
	DateTime newDate = date.AddDuration("P1Y");
	Console.WriteLine();
{% endhighlight %}
// TODO: finish this article

