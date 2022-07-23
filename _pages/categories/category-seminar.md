---
title: "Seminar"
layout: archive
permalink: categories/seminar
author_profile: true
sidebar_main: true
---

부트캠프를 수강하며,        
지식 공유를 위해 다른 수강생과 자체적으로 세미나를 열어 발표했던 자료들의 기록입니다.

{% assign posts = site.categories.seminar %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}