---
layout: default
title: Performance Budget - Documentation - sitespeed.io
description: Performance budget with sitespeed.io.
keywords: performance, budget, documentation, web performance, sitespeed.io
author: Peter Hedenskog
nav: documentation
image: https://www.sitespeed.io/img/sitespeed-2.0-twitter.png
twitterdescription: Performance budget with sitespeed.io.
---
[Documentation]({{site.baseurl}}/documentation/sitespeed.io/) / Performance Budget

# Performance Budget
{:.no_toc}

* Lets place the TOC here
{:toc}

## Performance budget
Have you heard of a performance budget? If not, please read the excellent posts by Tim Kadlec [Setting a performance budget](http://timkadlec.com/2013/01/setting-a-performance-budget/) and [Fast enough](http://timkadlec.com/2014/01/fast-enough/). Also read Daniel Malls [How to make a performance budget](http://danielmall.com/articles/how-to-make-a-performance-budget/). After that, continue setup sitespeed.io :)


### How it works
When you run sitespeed.io configured with a budget, the script will exit with a exit status > 0 if the budget fails. It will log the budget items that are failing and the ones that are working, and create a HTML report for the budget.

The log will look something like this:

~~~
[2016-10-24 10:53:01] Failing budget pagexray.pageSummary.transferSize for https://www.sitespeed.io/ with value 184.7 KB max limit 97.7 KB
[2016-10-24 10:53:01] Failing budget pagexray.pageSummary.contentTypes.image.transferSize for https://www.sitespeed.io/ with value 157.3 KB max limit 97.7 KB
[2016-10-24 10:53:01] Failing budget coach.pageSummary.advice.info.domElements for https://www.sitespeed.io/ with value 215 max limit 200
[2016-10-24 10:53:01] Failing budget coach.pageSummary.advice.info.domDepth.max for https://www.sitespeed.io/ with value 11 max limit 10
[2016-10-24 10:53:01] Budget: 8 working and 4 failing tests
~~~

<!--
And the report looks like this.
![Example of the budget](budget.png)
{: .img-thumbnail}
-->

Lets see how you configure your budgets.


### The budget file
The current version can handle min/max values and works on the internal data structure.

~~~
{
  "browsertime.pageSummary": [{
    "metric": "statistics.timings.firstPaint.median",
    "max": 1000
    }, {
    "metric": "statistics.visualMetrics.FirstVisualChange.median",
    "max": 1000
  }],
  "coach.pageSummary": [{
    "metric": "advice.performance.score",
    "min": 75
  }, {
    "metric": "advice.info.domElements",
    "max": 200
  }, {
    "metric": "advice.info.domDepth.max",
    "max": 10
  }, {
    "metric": "advice.info.iframes",
    "max": 0
  }, {
    "metric": "advice.info.pageCookies.max",
    "max": 5
  }],
  "pagexray.pageSummary": [{
    "metric": "transferSize",
    "max": 100000
  }, {
    "metric": "requests",
    "max": 20
  }, {
    "metric": "missingCompression",
    "max": 0
  }, {
    "metric": "contentTypes.css.requests",
    "max": 1
  }, {
    "metric": "contentTypes.image.transferSize",
    "max": 100000
  },{
    "metric": "documentRedirects",
    "max":0
  }]
}

~~~
Then run it like this:

~~~bash
$ sitespeed.io https://www.sitespeed.io/ --budget myBudget.json -b chrome -n 11
~~~