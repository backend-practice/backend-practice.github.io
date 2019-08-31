---
layout: default
---

{% for post in site.posts %}
## [{{post.title}}]({{site.baseurl}}{{post.url}})
*{{post.date|date_to_string}}*
{{post.excerpt}}
{% endfor %}
