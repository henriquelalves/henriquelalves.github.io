+++
title = "spring lisp game jam - devlog 4"
date = 2024-04-23
[taxonomies]
tags = ["lisp", "guile", "gamejam", "devlog"]
+++

I read a bit of Janet documentation and explored the amazing [Janet for Mortals](https://janet.guide/) book, and I think I might have to sit on this problem for a while.

The FFI solution I mentioned before is less than ideal because:

1. I'll have to wrap the **boring** parts of the engine to the scripting language (e.g. managing windows, managing the lifetime of objects). I want to be able to `load-texture` in the scripting language and **not worry about unloading it**, because its lifetime will be binded by the boring engine. This is my ideal boring engine & exciting gameplay scripting environment.
2. I don't want to wrap the game from the scripting language, I want to wrap it from C. Creating an FFI for the "boring" bits of the engine means that the C part of the engine is a client for the scripting language I'm using, not the other way around, and I have a feeling this will make exporting the engine to other platforms much trickier and dependant on a language that certainly has less support than C.

This way of thinking about the engine might be related to my experience using Godot and its scripting language GDScript. Godot does this really well, and I wanted to replicate this experience in a **much** smaller editor-less toy engine.

From problem 1, the simplest scenario of using `LoadTexture` and `UnloadTexture` API from RayLib means I need to bind the lifetime of a GC object (scripting environment world) to C. Guile `finalizers` would do the trick - but then I'd need to solve the much more arcane problem of Guile's integration with RayLib. Janet doesn't seem like it has the same GC hooks, so unless I think on an alternative for binding the lifetime of textures, it's out-of-question.
