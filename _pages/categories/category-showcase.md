---
title: "Showcase"
layout: archive
permalink: categories/showcase
author_profile: true
sidebar_main: true
---

수강 기간 중 있었던 주요 성과를 정리한 곳입니다.

{% assign posts = site.categories.showcase %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}