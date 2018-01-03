{% for post in site.posts limit:1 %}
# [{{ post.title }}]({{ site.baseurl }}{{ post.url }})
<p style="text-align:right; font-style:italic;">{{ page.date | date: '%B %d, %Y' }}</p>
<div>{{ post.content }}</div>
{% endfor %}

---
## Recent Posts
{% for post in site.posts offset:1 limit:10 %}
- [{{ post.title }}]({{ site.baseurl }}{{ post.url }}){% endfor %}
