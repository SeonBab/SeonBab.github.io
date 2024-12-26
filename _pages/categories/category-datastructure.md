---
title: "자료구조"
layout: archive
permalink: categories/datastructure
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.DataStructure = site.posts | sort: "order" | reverse %}
{% for post in posts %}
    {% include archive-single2.html type=page.entries_layout %}
{% endfor %}