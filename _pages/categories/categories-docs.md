---
title: "Documents"
layout: archive
permalink: categories/docs
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.docs %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}