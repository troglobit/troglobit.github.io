---
layout: page
title: Projects
permalink: /projects/
---

<dl class="tags-box">
{% for page in site.projects %}

{% if page.name %}
<dt><a class="page-link" href="{{ site.baseurl }}{{ page.url }}" title="{{ page.title }}">{{ page.name }}</dt>
{% if page.title %}
<dd>{{ page.title }}</dd>
{% endif %}
{% endif %}

{% endfor %}
</dl>
