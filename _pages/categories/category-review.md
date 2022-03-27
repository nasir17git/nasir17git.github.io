---
title: "Reviews"
layout: archive
permalink: categories/review
author_profile: true
sidebar_main: true
---

주간 수업을 듣고 익힌 내용을 취합정리하였습니다.

{% assign posts = site.categories.review %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}