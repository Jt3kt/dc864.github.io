<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }} {{ post.date | remove: "00:00:00 +0000" }}</a>
    </li>
  {% endfor %}
</ul>
