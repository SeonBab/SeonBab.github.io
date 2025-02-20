---
title: "알고리즘 문제 ETC"
layout: archive
permalink: categories/AC_ETC
author_profile: true
sidebar_main: true
---

{% assign posts = site.categories.AC_ETC = site.posts | sort: "order" | reverse %}
{% for post in posts %}
    {% include archive-single2.html type=page.entries_layout %}
{% endfor %}