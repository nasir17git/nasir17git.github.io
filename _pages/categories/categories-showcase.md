---
title: "Showcase"
layout: archive
permalink: categories/showcase
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.showcase %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}