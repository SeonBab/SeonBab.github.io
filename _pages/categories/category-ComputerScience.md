---
title: "컴퓨터 과학"
layout: archive
permalink: categories/ComputerScience
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.ComputerScience = site.posts | sort: "order" | reverse %}
{% for post in posts %}
    {% include archive-single2.html type=page.entries_layout %}
{% endfor %}