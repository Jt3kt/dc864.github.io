---
layout: default
---
<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }} {{ post.date | slice: 0,10 }}</a>
    </li>
  {% endfor %}
</ul>
