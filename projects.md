---
title: Projects
permalink: /projects/
---

A list of projects in which I took part or I am still producing.

comeca
{% for repository in site.github.public_repositories %}
  * [{{ repository.name }}]({{ repository.html_url }})
{% endfor %}

{{ repository }}
{{ site.github }}
{{ site }}

termina
