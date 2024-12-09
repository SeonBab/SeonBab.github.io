---
title: "GitHub"
layout: archive
permalink: categories/github
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.GitHub = site.posts | sort: "order" | reverse %}
{% for post in posts %}
    {% include archive-single2.html type=page.entries_layout %}
{% endfor %}