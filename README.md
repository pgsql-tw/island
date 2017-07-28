{% for post in site.posts limit:1 %}
<div>{{ post.content }}</div>
{% endfor %}
### Recent Posts
{% for post in site.posts offset:1 limit:10 %}
   * ({{post.title}})[{{post.url}}]
{% endfor %}
