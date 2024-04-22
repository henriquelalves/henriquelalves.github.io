+++
title = "spring lisp game jam - devlog 3"
date = 2024-04-22
[taxonomies]
tags = ["lisp", "guile", "gamejam", "devlog"]
+++

After some exploration and learning a little bit more about Guile, it looks like I found a major problem with my integration.

The idea was to create a small C game engine in RayLib that integrated Guile by calling two named scheme methods from its game loop: `update` and `draw`. Those Guile methods would call the C Raylib wrappers I created, so the Guile script would be this simple script calling RayLib C functions via the update and draw entrypoints.

Alas, it seems Guile doesn't like this approach. I managed to create a simple wrapped "Texture2D" structure to use with Guile, and the idea was that I'd be able to just load a RayLib Texture2D in Guile, and the "unloading" would be done via a Guile Finalizer in the C world. But the Guile GC wasn't being called, I was getting memory leaks all over the place, and I couldn't find in the documentation any alternatives for the wrapping structs or functions.

I reduced the problem to a simple RayLib call of "DrawText". A direct call of DrawText in the C world always works; but when I do my little integration shenanigan (C-world calling Scheme's world "update" -> Scheme's world "update" calling C-world wrapped function), the follow happens:

- `DrawText("")` (drawing an empty string) segfaults.
- `DrawText("a")` (single character string) segfaults.
- `DrawText("aa")` (double character string) works normally (?).

And increasing the number of characters in the string might (or might not!) segfault and crash the game (fun fact, I discovered this bug by printing a number from 0 to 1000 - when it reached 1000, the game segfaulted because of the new number of characters in the string).

The MVP of the problem is here: [https://github.com/henriquelalves/SLGJ-2024](https://github.com/henriquelalves/SLGJ-2024).

There is the possibility of me making a really stupid mistake somewhere in the integration, but this is too close of a low-level high-effort arcane mistake that I don't want to worry about during the Jam. So, time to think on a plan B:

1. Drop this way of integrating Guile and Raylib, and follow `dthompson` wise advice on Guile's IRC: "most of us tend to recommend writing less C (preferably none) and using scheme to call C libraries when needed. all of my game programming stuff has been through the ffi so I know it's feasible."
2. Just don't use a low-level language at all. Do everything on Guile.
3. Replace Guile with [Janet](https://janet-lang.org/) on the same integration.

To be honest, I think I'll go with option 3 for now, just because I want to compare Janet's integration with Guile. Janet is less known and has less support, but who knows? Maybe I found my dream LISP integration for game development.
