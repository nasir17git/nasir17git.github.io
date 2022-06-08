---
title: "etc"
layout: archive
permalink: categories/etc
author_profile: true
sidebar_main: true
---

추가적으로 공부한 내용을 모아두었습니다.

{% assign posts = site.categories.etc %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}