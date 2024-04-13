+++
title = "spring lisp game jam - devlog 1"
date = 2024-04-12
[taxonomies]
tags = ["lisp", "guile", "gamejam", "devlog"]
+++

This is a quick devlog so I can track the state of development of my small Lisp Game Engine I'm building for the [Spring Lisp Game Jam](https://itch.io/jam/spring-lisp-game-jam-2024).

### The State Before

I've manage to build a small PoC for the engine using Raylib (C) as the backend part of the engine, and Guile for the scripting logic. The engine is 2D only, and will work with a similar API as [Pyxel](https://enginesdatabase.com/engine/pyxel/).

The current main question is whether I can make an **online build** using EMScripten to compile the Engine + Guile wrapper to web. My main question right now is **How do I compile libguile to Web?**

I'm currently using CMake to build the engine. I'm not very experience with CMake, so I decided I'd try to compile "manually" (using EMSdk tools) the Guile repository. My assumption is that I'd be able to compile libguile with EMsdk into libguile.h + EMSdk-compatible object files, and I'd be able to manually link those files with the main project EMSdk-enchanced CMake.

### Progress Today

Compiling the Guile repo using `emconfigure ./configure` + `emmake make` didn't work.

- First, `emconfigure ./configure` didn't work because GNU MP was missing (although GNU MP lib is installed). My understandment is that a "missing" library is a library that isn't "included" with EMSdk, so I'd need to compile it using EMSdk tools and link the new compatible version. I could circumvent this error using `emconfigure ./configure --enable-mini-gmp`.
- Then, I got an error that GNU libunistring was also missing. This error gave me no options to replace the library, so I cloned GNU libunistring repository to test compiling it using EMSdk.
- Before running `./autogen.sh`, I had to run `./gitsub.sh pull` to download some repository dependencies. Still, running autogen after that gave me a bunch of errors and warnings. Running `emconfigure ./configure`seemed ok until the following error:

`config.status: error: cannot find input file: 'lib/Makefile.in'`

and then, running `emmake make` gave me:

`/Users/macpc/Projects/libunistring/build-aux/missing: line 81: makeinfo: command not found`

A `a.wasm` file was still generated (**why? Does EMSdk use wasm files instead of '.o' files?**), along with a `gnulib/` folder. Interesting enough, the wasm file was created on `emconfigure ./configure`, so I really have no idea why it is useful.


### Next Steps

I feel I have a better grasp on how deep the "compile Guile with EMSdk" rabbit-hole goes, although I'm operating on some compilation assumptions I'm not quite sure (what EMSdk uses to link libraries? How can I use **some** libraries with EMSdk but not others?).

Worst-case scenario I can drop the idea of compiling the engine to web, but I believe now I need to:

1. Understand if I'm operating on the right assumptions.
2. Insist a bit more on compiling Guile to web by compiling each dependency. Right now, I'm stuck at `libunistring`.
