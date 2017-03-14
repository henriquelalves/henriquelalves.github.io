---
title: Godot
permalink: /godot/
main_menu: true
---

Godot is a free-software Game Engine that I self-proclaimed to be an evangelist for a couple of reasons:

- **It has a permissive license**; I love free-software and I love game-development; Godot is the perfect intersection of both.
- **Has a beautiful, easy to understand architecture**; the fact that Godot is heavily based on composition and scene-tree structure made me fall in love with it. And unlike other proprietary game-engines, it has a clear and functional script language and back-end language.
- **Has a growing community**; Godot is powerful enough of an Engine to atract other Developers, and has a steady development pace that just make features keep coming.

For those and dozens more reasons, Godot is my main Game-Engine software; so it only makes sense that I try my best to contribute for it. Here is the portfolio of the stuff that I made; hopefully it will be useful for other developers! (Click on the image to check on the project)

<table>
{% for project in site.godot_gallery %}
<div class="gallery">
  <a target="_blank" href="{{project.link}}">
    <img src="{{project.image}}" alt="{{project.name}}" width="500" height="300">
  </a>
  <div class="desc">
      <b>{{project.name}}</b><br>
      {{project.description}}
  </div>
</div>
{% endfor %}
</table>

