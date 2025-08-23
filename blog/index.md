---
layout: default
title: Blog
permalink: /blog/
---

# Welcome to My Cybersecurity Blog

Here you’ll find detailed walkthroughs of CTF challenges, labs, and exploit writeups covering network security, web security, and more. Explore each post to see step-by-step guides and lessons learned.

---

<div class="blog-container">
{% for post in site.posts %}
  <div class="blog-card">
    <h2><a href="{{ post.url }}">{{ post.title }}</a></h2>
    <p class="blog-date">{{ post.date | date: "%B %d, %Y" }}</p>
    <p class="blog-excerpt">{{ post.excerpt }}</p>
    <a href="{{ post.url }}" class="read-more">Read More →</a>
  </div>
{% endfor %}
</div>
