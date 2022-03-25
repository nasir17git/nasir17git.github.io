---
title: "Showcase"
layout: archive
permalink: categories/showcase
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.til %}
{% for post in posts %} {% include archive-single.html type=page.entries_layout %} {% endfor %}