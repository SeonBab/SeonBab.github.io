---
title: "스파르타 TIL"
layout: archive
permalink: categories/spartaTIL
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.SpartaTIL = site.posts | sort: "order" | reverse %}
{% for post in posts %}
    {% include archive-single2.html type=page.entries_layout %}
{% endfor %}