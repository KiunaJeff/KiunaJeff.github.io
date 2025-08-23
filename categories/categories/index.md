---
layout: default
title: Categories
permalink: /categories/
---

# Categories

Here you can browse posts by category.  

{% assign categories_list = site.categories | sort %}
<ul>
  {% for category in categories_list %}
    <li>
      <a href="{{ '/categories/#' | append: category[0] | relative_url }}">
        {{ category[0] }} ({{ category[1].size }})
      </a>
    </li>
  {% endfor %}
</ul>
