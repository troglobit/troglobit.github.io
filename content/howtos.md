---
layout: page
title: HowTos
permalink: /howtos/
---

<dl class="tags-box">
{% for page in site.howtos %}

{% if page.name %}
<dt>
  <a class="page-link" href="{{ site.baseurl }}{{ page.url }}" title="{{ page.title }}">{{ page.name }}</a>
</dt>
{% if page.title %}
<dd>
  {{ page.title }}
</dd>
{% endif %}
{% endif %}

{% endfor %}
</dl>
