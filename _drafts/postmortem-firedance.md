---
title: "Postmortem: LudumDare 36 Fire Dance"
lead: The development analysis of the simple NES-like game I tried creating during LudumDare 36
---

Theme of the jam: Anscient Technology.
I thought "what is the ansient-est technology I know of?" : Fire

NES game: simple mechanics, visual shader, color limitations.

Development in Godot;
- first time trying a development process of iterating EACH scene individually, debugging them until I found what I was looking for.
- Problems with Physics; couldn't disable them the way I wanted.
- NES limitations and creativity.
- Was looking for some "Ice Climbers" kind of look.
- Update problem on HUD? would be able to update score using a signal, but would rather just check on the global script by update.

- (Butterfly) Area2D collision check doesn't work without Fixed Process (tried creating recursive blink method)

- Finding the fun
    - Goat pacification mechanic was dull, and "hard" to explain. Than I thought about the butterfly mechanic.

Godot implementations:
- Disabling physics but with area collision
- Screen resizing without full-screen (distorted screen)
- Be able to insert sign '+' before numbers
- Problem in the Alpha of pixelated images
- small issue: commented line on the end with a ":" won't have extra tab when enter

Plugin Menu Options:
- Nodes created in tool mode are invisible in editor
- Multiline string can't create new lines through simple inline export editor.
- In tool script, setget methods are played before node is created.
