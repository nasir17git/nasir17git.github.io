---
title: "Showcase"
layout: archive
permalink: categories/showcase
author_profile: true
sidebar_main: true
---

수강 기간 중 발생한 주요 성과를 정리한 곳입니다.
차후 Grid gallery 형식으로 변경 예정

{% assign posts = site.categories.showcase %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}