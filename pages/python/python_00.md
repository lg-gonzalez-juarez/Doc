---
title: 00. Course Index
tags: [python]
keywords: trainning, courses
last_updated: January 14th, 2021
summary: "This shows a review of mainly points about python that I have considered"
sidebar: python_sidebar
permalink: python_00.html
toc: false
folder: python
---
This pages shown the fundamentals of python required to take and pass the **certified Entry-Level Python Programmer Certification** exam. 
This was extracted from [Linux Academy Course](https://linuxacademy.com/cp/modules/view/id/413?redirect_uri=https://app.linuxacademy.com/search?query=entry%20level%20programmer%20certification)
This covers:

<!-- List of all pages with a python tag -->

<ul>
{% assign sorted_pages = site.pages | sort: 'title' %}
{% for page in sorted_pages %}
{% for tag in page.tags %}
{% if tag == "python" %}
<li><a href="{{ page.url | remove: "/"}}">{{page.title}}</a></li>
{% endif %}
{% endfor %}
{% endfor %}
</ul>


## Hands-on Labs

<ul>
{% assign sorted_pages = site.pages | sort: 'title' %}
{% for page in sorted_pages %}
{% for tag in page.tags %}
{% if tag == "handson" %}
<li><a href="{{ page.url | remove: "/"}}">{{page.title}}</a></li>
{% endif %}
{% endfor %}
{% endfor %}
</ul>

