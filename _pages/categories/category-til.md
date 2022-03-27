---
title: "TIL(Today I Learned)"
layout: archive
permalink: categories/til
author_profile: true
sidebar_main: true
---

일간 수업을 듣고 이해한 내용을 정리하였습니다.

{% assign posts = site.categories.til %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}