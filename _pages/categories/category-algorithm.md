---
title: "알고리즘"
layout: archive
permalink: categories/algorithm
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.Algorithm = site.posts | sort: "order" | reverse %}
{% for post in posts %}
    {% include archive-single2.html type=page.entries_layout %}
{% endfor %}