---
title: "git"
layout: archive
permalink: categories/git
author_profile: true
sidebar_main: true
---

Git에 대해서 공부하고,     
Git blog 기능 설정 방법에 대해 정리하였습니다.

{% assign posts = site.categories.git %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}