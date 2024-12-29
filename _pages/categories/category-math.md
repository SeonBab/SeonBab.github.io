---
title: "수학"
layout: archive
permalink: categories/math
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.Math = site.posts | sort: "order" | reverse %}
{% for post in posts %}
    {% include archive-single2.html type=page.entries_layout %}
{% endfor %}