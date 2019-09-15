---
layout: default
---

## Stories

<ul class="post">
  {% for post in site.posts %}
    <article class="story">
      <h4><a href="{{ post.url }}">{{ post.title }}</a></h4>
      <p><i class="glyphicon glyphicon-time"></i> {{ post.read }} read</p>
      <p>
        <i class="glyphicon glyphicon-tag"></i>
        {% for category in post.categories %}
          {{ category }}
        {% endfor %}
      </p>
      {% for tag in post.tags %}
        <p>&emsp;&emsp;<i class="glyphicon glyphicon-chevron-right"></i> {{ tag }}</p>
      {% endfor %}
    <blockquote>
      <p>{{ post.firstLine }}</p>
    </blockquote>

    </article>
  {% endfor %}
</ul>


[back](../)
