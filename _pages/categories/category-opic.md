---
title: "Showcase"
layout: archive
permalink: categories/opic
author_profile: true
sidebar_main: true
---

OPIC 시험을 준비하며 찾아봤던 내용과 준비했던 과정을 기록하였습니다.

갱신기간에 다시보고 어떤걸 했었는지 확인할 용도로 보존

{% assign posts = site.categories.opic %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}