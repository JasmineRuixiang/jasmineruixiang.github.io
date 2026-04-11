---
layout: page
permalink: /PhiarLitSic/
title: PhiArLitSic
nav: true
nav_order: 2
description: This is the page for displaying my thoughts and works on philosophy, art, literature, and music.
---

<div class="row mb-5 mt-5">
  <div class="col-sm-12">
    <h2 class="category">Music</h2>
    <hr>
    <iframe 
      width="100%" 
      height="450" 
      scrolling="no" 
      frameborder="no" 
      allow="autoplay" 
      src="https://w.soundcloud.com/player/?url=https%3A//soundcloud.com/ruixiang-li&color=%23007bff&auto_play=false&hide_related=false&show_comments=true&show_user=true&show_reposts=false&show_teaser=true">
    </iframe>
  </div>
</div>

{% if site.data.music and site.data.music.size > 0 %}
<div class="row mt-4">
  <div class="col-sm-12">
    <h3 class="category">Highlighted Pieces</h3>
    <hr>
  </div>
</div>
<div class="row row-cols-1 row-cols-md-2">
  {% for piece in site.data.music %}
  <div class="col mb-4 d-flex">
    <div class="card w-100 hoverable">
      <div class="card-body d-flex flex-column">
        <h4 class="card-title">{{ piece.title }}</h4>
        {% if piece.release_date %}
          <h6 class="card-subtitle mb-2 text-muted">{{ piece.release_date }}</h6>
        {% endif %}
        <p class="card-text">{{ piece.description }}</p>
        <div class="mt-auto">
          {{ piece.embed_code }}
        </div>
      </div>
    </div>
  </div>
  {% endfor %}
</div>
{% endif %}