+++
title = "spring lisp game jam - devlog 6"
date = 2024-05-27
[taxonomies]
tags = ["lisp", "guile", "gamejam", "devlog"]
+++

Too long since my last devlog, and Lisp Game Jam already went by! The game I made was this one: [Life Predictor](https://illusion-fisherman.itch.io/life-predictor). It's not 100% stable (I have a segfault happening sometimes lol, lmao even), but it's a nifty simple Game of Life puzzle.

End result: I managed to finish the game on time, and I have a very clear image of the "feature-complete" engine in my head - but the engine itself is far from finished. The current game was made with Raylib (in C) and S7 Scheme for all gameplay scripts, and a very simple wrapper on some Raylib functions. What I managed to validate:

- S7 Scheme works QUITE well for what I have in mind. I can easily integrate it in C, its source code is very simple, I have easy access to its garbage collector events, and I can easily compile it to web!
- Evaluating code in runtime is quite easy, but there are some small shenanigans I have to be careful when I have pointers to Scheme code in C (e.g. using `set!` on a symbol that has a pointer in C makes won't change the memory location C has access to, so it will "see" the same data as before).
- Raylib was also really, really easy to integrate. I'm not sure I want to stick it till the end because there are some really interesting libs I wanted to test on rendering, UI and physics, so I may want to keep Raylib a bit modular and work on it's integration as time goes on.

Now that I validated the basics of my idea of the perfect Game Jam Engine, this are the features that I have in mind:

- REPL Server! Specifically, I want to be able to connect to the running game via Emacs Geiser. This is a must have for the feature.
- Save states for Scheme code. Ideally, I want the game you play to have a similar behavior than a retro-game emulator, with save states you can load anytime to iterate or test on specific parts of the gameloop.
- An Emacs mode for the game engine. It seems a bit too fancy, but the idea is to use the REPL Server to communicate with the game in runtime, and to use Emacs as an IDE to get/set values of any variables you see in the game scripts, know which code is different from the runtime environment, add new pieces of code in runtime, etc. Basically a REPL facilitator.

I'm pretty much sure this will take a lot of time to be done, but my plan is to keep using the engine on other game jams, and keep on iterating on it.

Last but not least, thanks for [Gil Barbosa](https://github.com/gilzoide) to help me out discussing the techstack with me and contributing in the project!
