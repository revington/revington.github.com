
---
layout: post
category : programming
tags : [c#, xml]
---
{% include JB/setup %}

Have you ever tried to convert from [xml duration](http://www.w3.org/TR/2001/REC-xmlschema-2-20010502/#duration) to TimeSpan and then add it to a DateTime?
{% highlight c# %}
	TimeSpan ts = System.Xml.XmlConvert.ToTimeSpan("P5Y");
	DateTime now = new DateTime(2008,2,29);
	Console.WriteLine(now + ts); // 27/02/2013 0:00:00
{% endhighlight %}
What?
Months and years vary in length but TimeSpan treats *ALWAYS* years as 365 days and months as 30.

## Fix

{% highlight c# %}
	DateTime now = new DateTime (2008, 2, 29);
	string duration = "P1Y";
	Regex expr = 
		new Regex (@"(-?)P((\d{1,4})Y)?((\d{1,4})M)?((\d{1,4})D)?(T((\d{1,4})H)?((\d{1,4})M)?((\d{1,4}(\.\d{1,3})?)S)?)?", RegexOptions.Compiled | RegexOptions.CultureInvariant);
	bool positiveDuration = false == (input [0] == '-');

	MatchCollection matches = expr.Matches (duration);
	var g = matches [0];
	Func<int,int> getNumber = x => {
		if (g.Groups.Count < x || string.IsNullOrEmpty (g.Groups [x].ToString ())) {
			return 0;
		}
		
		int a = int.Parse (g.Groups [x].ToString ());
		
		return PositiveDuration ? a : a * -1;
		
	};
	now.AddYears (getNumber (3));
	now.AddMonths (getNumber (5));
	now.AddDays (getNumber (7));
	now.AddHours (getNumber (10));
	now.AddMinutes (getNumber (12));
	now.AddSeconds (getNumber (14));
	Console.WriteLine (now); // 28/02/2012 0:00:00
{% endhighlight %}
