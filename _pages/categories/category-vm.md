---
title: "VMWare"
layout: archive
permalink: categories/vm
author_profile: true
sidebar_main: true
---

VMWare과 관련하여 공부한 기록 모음

- [Complete VMWare vSphere ESXi and vCenter Administration](https://www.udemy.com/course/complete-vmware-vsphere-esxi-and-vcenter-administration/)

{% assign posts = site.categories.vm %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}