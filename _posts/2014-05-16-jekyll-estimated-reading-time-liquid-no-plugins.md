---
layout: post
published: true
title: "Jekyll Estimated Reading Time: Liquid, no plugins"
mathjax: false
featured: false
comments: false
description: Implementing Medium inspired Estimated Reading Time (ERT) Calculator
categories: 
  - webdevelopment
---

The first time I discovered Medium, I fell in love with its Estimated Reading Time (ERT) feature. It helps bring in the right audience to the right article. The idea behind of this is, maybe it’s early in the morning, you’re in the mood of reading something terse while you get prepared for work, and at dawn, you’re back at home and you just want to curl up on the couch and gorge in a long essay. The ERT helps you find the right article, for the right time.

So with that in mind, I looked on the internet if someone has already made a code-snippet for this. Sure enough, I found a Jekyll plugin that does it right. But the problem was, I was and currently am hosting with GitHub pages, which does not allow external plugins. So I wrote my own code in Liquid.
First the calculator:

{% highlight ruby %}
{% raw %}
{% assign ert = page.content | strip_html | number_of_words | divided_by:180 %}
{% assign ertremainder = page.content | strip_html | number_of_words | modulo:180 %}
{% if ertremainder >= 90 %}
    {% assign readtime = ert | plus:1 %}
{% else %}
    {% assign readtime = ert %}
{% endif %}
{% endraw %}
{% endhighlight %}

The idea behind it is that people can read 180 words per minute on average. So if I take the page content, strip off the html, count the number of words and divide it with 180, I will get the rough estimate of how long it will take an average reader to go through the corresponding article.

To fine tune the results, I also take the remainder from the division. The idea is that, if the read time is more than halfway through the minute, the ERT assigns the next corresponding minute as the read time estimate.

For displaying the data to the user, I was interested in 3 different scenarios. If the readtime is less than 30 seconds, I wanted to display “Less than 1 minute read”, if the readtime is in between 30 to 60 seconds, the ERT shows “1 minute read”, and in all the cases where the readtime is more than 1 and half minutes, I wanted to display the calculated read time estimate in “minutes”. The redundancy is to ensure that the pluralization of the word “minute” is being done properly.

{% highlight ruby %}
{% raw %}
{% if site.readtime %}
<i class="icon-time"></i>&nbsp;
	{% if readtime == 1 %}1 minute read{% endif %}
	{% if readtime > 1 %}{{ readtime }} minutes read{% endif %}
	{% if readtime == 0 %}Less than 1 minute read{% endif %}
{% endif %}
{% endraw %}
{% endhighlight %}

## **Requests?**

If you have some specific requests for this snippet, or if you need help custom coding, message me on Twitter [@hmfaysal](http://twitter.com/hmfaysal) or email me at [hmfaysal@alum.mit.edu](mailto:hmfaysal@alum.mit.edu)

If you'd like to give me credit somewhere on your blog or tweet a shout out to [@hmfaysal](https://twitter.com/hmfaysal), that would be pretty sweet.