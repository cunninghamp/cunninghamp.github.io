---
title: "Welcome"
permalink: "/"
layout: splash

header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: /assets/images/home-splash.jpg
  actions:
    - label: "Read Posts"
      url: "/posts/"

excerpt: "I'm not a developer. My work is at the intersection of business management, marketing, and technology. I write code to solve problems and answer questions. What I learn along the way, I'll share here."
---

<h2>The latest...</h2>
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>