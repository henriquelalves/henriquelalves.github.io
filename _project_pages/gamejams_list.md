---
title: Gamejams List
permalink: /gamejams_list
layout: page
---

A list of most GameJams (with the corresponding games) that I participated:

{% for jam in site.gamejams_list %}
* [{{ jam.game }} _({{ jam.name }})_]({{ jam.link}} ): 
{{ jam.description }}{% if jam.postmortem %} [Postmortem analysis!]({{ jam.postmortem }}){% endif %}
{% endfor %}
