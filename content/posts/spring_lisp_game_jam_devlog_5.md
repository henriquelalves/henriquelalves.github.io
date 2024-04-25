+++
title = "spring lisp game jam - devlog 4"
date = 2024-04-25
[taxonomies]
tags = ["lisp", "guile", "gamejam", "devlog"]
+++

Finally a breakthrough!

Last night I was trying alternatives for Guile, and I stumbled upon [S7 scheme](https://ccrma.stanford.edu/software/snd/snd/s7.html) from a Reddit post. It had the two main things I was looking for:

- Super simple and sane C FFI API.
- Garbage Collector hooks so I could free memory from C allocated objects.

I found S7 actually even simpler than Guile! Its API was easy to understand, and even wrapping structs to S7 was very straightforward - I didn't even had to create a different wrapping structure for loading a Texture2D, I just needed to allocate the Texture2D on the heap and "UnloadTexture" on the garbage collector hook. The simplest scenario that didn't work on Guile worked on S7 without having to create workarounds!

The one thing that I didn't understand was the Garbage Collector itself. I created a simple loop that loaded textures every frame to test when the Garbage Collector would call my hook to free the previously allocated textures, but the GC was never called at all. I suppose the way I'm calling S7 scheme functions from the C code disables the GC while the call is ongoing, and since I only ever have those two calls to S7 (my `update` and `draw` functions), the GC never have time to be called. That almost works in my favour though - calling the GC is quite easy (even from scheme I can just call `(gc)`) and my GC hook to free the loaded Texture2Ds did work. That means I can create my own GC calling policy, so it's not called in the middle of the gameplay.

A bonus breakthrough was checking how to build S7 for Web (from S7 docs), and I managed to make the project work on the browser, but for now **without loading scripts** (starting the S7 environment apparently did work though!). This makes sense - I never expected that loading scripts would actually work on the browser. What I **think** I might have to do is to compile the Scheme code to a C object, and link the object during the build process (but only for the Web build). Or something like that, I don't know, it's my first time dealing with CMake too.

Too many new things, but progress has been made!
