---
layout: page
title: "Blog"
permalink: /blog/
---

# Blog

Welcome to my cybersecurity blog!  
Here you’ll find detailed walkthroughs of CTF challenges, labs, and exploit writeups.

---

<ul>
{% for post in site.posts %}
  <li>
    <a href="{{ post.url }}">{{ post.title }}</a> — {{ post.date | date: "%B %d, %Y" }}
  </li>
{% endfor %}
</ul>
