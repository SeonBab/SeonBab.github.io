---
title: "디자인 패턴"
layout: archive
permalink: categories/designpattern
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.DesignPattern = site.posts | sort: "order" | reverse %}
{% for post in posts %}
    {% include archive-single2.html type=page.entries_layout %}
{% endfor %}