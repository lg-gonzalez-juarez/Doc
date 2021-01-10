---
title: Python_intro
tags: [python]
keywords: trainning, courses
last_updated: January 3th, 2021
summary: "This shows a review of mainly courses that I have considered"
sidebar: mydoc_sidebar
permalink: python_00.html
toc: false
folder: python
---


## List of all pages with a python tag

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

{% include links.html %}
