---
title: "Cpp"
layout: archive
permalink: categories/cpp
author_profile: true
sidebar_main: true
is_sortable: true
---

{% assign posts = site.categories.Cpp | sort: 'title' | reverse %}
{% for post in posts = site.pages %} 
    {% include archive-single2.html type=page.entries_layout %}
{% endfor %}