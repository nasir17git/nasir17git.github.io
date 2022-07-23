---
title: "Python"
layout: archive
permalink: categories/Python
author_profile: true
sidebar_main: true
---

Python에 대해 공부하며 정리가 필요한 자료들을 모아두었습니다.    

{% assign posts = site.categories.python %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}