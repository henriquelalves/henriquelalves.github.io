+++
title = "spring lisp game jam - devlog 2"
date = 2024-04-13
[taxonomies]
tags = ["lisp", "guile", "gamejam", "devlog"]
+++

After a good night sleep, I now believe that the game engine I have in mind is NOT compatible for web, and any solution I find will be a workaround the design of the architecture I want.

The idea was to create a Game Engine that separates the boring part (rendering, window management, input) from the fun part (game logic) by creating two well-integrated codebases:

- Raylib (C lang) as backend for everything boring. This part of the codebase is compiled in an executable.
- Guile as scripting language for all game logic. The scripts will be exported with the executable, but they should live in a folder as-is and should be easy to change.

Most game engines that export to web pack the entire application into a Wasm executable, which is great for browser compatibility but it's fundamentally different from what I want from my game engine. The main reason I chose Guile is that it's a scripting language with amazing compatibiltity to the C backend I have in mind, but compiling <libguile.h> to Web means I'm basically killing the user-side functionality of a scripting language in the first place.

So, what to do? The alternatives are:

- **Use [a] Lisp that can export to Web.** There are some Lisps and frameworks like [Guile Hoot](https://spritely.institute/news/guile-hoot-v040-released.html) that can export to Web. This is a fundamentally different game engine than the one I wanted to experiment with (no clear boundaries of boring/fun parts of the engine), but it's a compromise if I only want to learn Lisp.
- **Use a tech-stack better compatible with Web and sacrifice user scripting.** I could use JVM [LibGDX](https://enginesdatabase.com/engine/LibGDX/) + Clojure to create a similar architecture of Boring VS Fun engine components and export it MUCH easier to Web (supposedly! I never used LibGDX). I'd sacrifice user scripting in the process.
- **Keep insisting on compiling <libguile.h> to web using EMScripten.** This is still useful (as a learning opportunity) and it would allow me to create a Game Engine to Rule them All (and export the same game to the Web, and executables with user scripting allowed). In the long-term this seems like a good solution, but right now I have to many unknown-unknowns with C and Guile programming + using EMSdk to compile them to Web.

So... I think I'll just give up doing a Web version of the game for Spring Lisp Game Jam, and focus on building a good framework for the game engine that runs locally on player machines. I can explore the other solutions on other game jams.

My next steps will be to start exploring the C Raylib wrapper and play around the Guile integration. No more EMSdk shenanigans for this jam!
