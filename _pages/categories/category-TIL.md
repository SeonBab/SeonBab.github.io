---
title: "TIL"
layout: archive
permalink: categories/TIL
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.TIL = site.posts | sort: "order" | reverse %}
{% for post in posts %}
    {% include archive-single2.html type=page.entries_layout %}
{% endfor %}