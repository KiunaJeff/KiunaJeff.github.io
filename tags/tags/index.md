---
layout: default
title: Tags
permalink: /tags/
---

# Tags

Here you can browse posts by tag.  

{% assign tags_list = site.tags | sort %}
<ul>
  {% for tag in tags_list %}
    <li>
      <a href="{{ '/tags/#' | append: tag[0] | relative_url }}">
        {{ tag[0] }} ({{ tag[1].size }})
      </a>
    </li>
  {% endfor %}
</ul>
