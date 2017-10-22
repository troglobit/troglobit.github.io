---
layout: page
title: Projects
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
<dt>
  <a class="page-lin" href="http://merecat.troglobit.com" title="Merecat httpd">Merecat</a>
</dt>
<dd>
  Simple, small and fast HTTP server
</dd>
</dl>
