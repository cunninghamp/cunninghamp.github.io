---
title: Welcome
layout: custom-home
permalink: /
---

<div id="main" role="main">
  {% include sidebar.html %}

  <div class="archive">
    {% unless page.header.overlay_color or page.header.overlay_image %}
      <h1 id="page-title" class="page__title">{{ page.title }}</h1>
    {% endunless %}
    {{ content }}
  </div>
</div>

<p>I'm not a developer. My work is at the intersection of business management, marketing, and technology. I write code to solve problems and answer questions. What I learn along the way, I'll share here.</p>