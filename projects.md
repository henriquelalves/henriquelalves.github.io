---
title: Projects
permalink: /projects/
main_menu: true
---

A list of projects in which I took part or I am still producing.

### Games
A good fraction of my games were made during GameJams; a list of most Jams in which I participated (with the corresponding games) is [here](/gamejams_list/).

### GitHub hosted Projects
For more projects, visit [my GitHub page](https://github.com/henriquelalves).

<!--- Repository names I want to make a list with -->
{% assign reponames = "GodotTIE|SimpleGodotCRTShader|Chaos|Genetic-Programming-Predator-Prey-Simulation|Polycode-SkeletonSurvival|Ambigram|tic-tac-turn" | split: "|" %}

<!--- Repositories list! -->
{% for repository in site.github.public_repositories %}
{% for name in reponames %}
{% if repository.name == name %}
[{{ repository.name }}]({{ repository.html_url }}): {{ repository.description }} _(Stargazers count: {{ repository.stargazers_count }})_
{% endif %}
{% endfor %}
{% endfor %}
