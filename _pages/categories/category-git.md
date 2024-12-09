---
title: "Git"
layout: archive
permalink: categories/git
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.Git = site.posts | sort: "order" | reverse %}
{% for post in posts %}
    {% include archive-single2.html type=page.entries_layout %}
{% endfor %}