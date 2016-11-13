---
layout: page
title: Projects
permalink: /projects/
---

<dl class="tags-box">
{% for page in site.projects %}

{% if page.name %}
<dt>
  <a class="page-link" href="{{ site.baseurl }}{{ page.url }}" title="{{ page.title }}">{{ page.name }}</a>
</dt>
{% if page.description %}
<dd>
  {{ page.description }}
</dd>
{% else if page.title %}
<dd>
  {{ page.title }}
</dd>
{% endif %}
{% endif %}

{% endfor %}
</dl>
