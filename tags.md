---
layout: page
title: Tags
permalink: /tags/
---

{%- assign tags = site.tags | sort -%}

{%- if tags.size > 0 -%}
<ul class="tag-cloud">
  {%- for tag in tags -%}
    {%- assign name = tag[0] -%}
    {%- assign posts = tag[1] -%}
    <li>
      <a href="{{ site.baseurl }}/tags/{{ name | downcase }}/">#{{ name }}</a>
      <span class="tag-count">{{ posts.size }}</span>
    </li>
  {%- endfor -%}
</ul>
{%- else -%}
<p>No tags yet.</p>
{%- endif -%}
