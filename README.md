{% for post in site.posts limit:1 %}
<div>{{ post.content |truncatehtml | truncatewords: 60 }}</div>
{% endfor %}
<h1>Recent Posts</h1>
{% for post in site.posts offset:1 limit:2 %}
... Show the next two posts ...
{% endfor %}
