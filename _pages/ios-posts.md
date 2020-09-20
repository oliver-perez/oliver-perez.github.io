---
layout: archive
permalink: /iOS-posts/
title: "iOS Posts by Tags"
author_profile: true
---

{% include base_path %}
{% include group-by-array collection=site.posts field="tags" %}

{% for tag in group_names %}
  {% assign posts = group_items[forloop.index0] %}
  <h2 id="{{ tag | slugify }}" class="archive__subtitle">{{ tag }}</h2>
  {% for post in posts %}
    {% includes archive-single.html %}
  {% endfor %}
{% endfor %}
