---
layout: default
title: Blog
permalink: /blog/
---

# Welcome to My Cybersecurity Blog

Here you’ll find detailed walkthroughs of CTF challenges, labs, and exploit writeups covering network security, web security, and more. Explore each post to see step-by-step guides and lessons learned.

---

{% for post in site.posts %}
  <article>
    <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
    <p><em>{{ post.date | date: "%B %d, %Y" }}</em></p>
    {{ post.content }}
    <hr>
  </article>
{% endfor %}
