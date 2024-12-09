---
title: "C++"
layout: archive
permalink: categories/cpp
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.Cpp = site.posts | sort: "order" | reverse %}
{% for post in posts %}
    {% include archive-single2.html type=page.entries_layout %}
{% endfor %}