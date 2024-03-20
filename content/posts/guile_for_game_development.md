+++
title =  "guile for game development - a devlog"
date = 2024-03-20
[taxonomies]
tags = ["guile", "devlog", "raylib"]
+++

Trying to synthesize a bit of my learning experience with Guile, so I can better track my own progress learning it.

### first, what do I want to do?

The project that I have in mind is to create a turn-based Tabletop Game Engine (TGE). The idea is that the language itself can describe rules and abstract the most common things used in tabletop games, and the Engine would enforce those rules, show which moves are available for the players on their turn, etc. I'm not worried about making the visuals for the Engine, just pure data - so in the future, I could embed the TGE anywhere, and the state of each turn would be an Engine query of "What is the current state of the board?", and I would draw it anyway I want.

This is **not a novel idea**. The thought actually came to me when out of curiosity I started studying a bit the [Game Description Language](https://en.wikipedia.org/wiki/Game_Description_Language), and I fell into a rabbit hole of declarative metagame descriptions and other experiments with similar objectives (this is worth another post with some very interesting findings). However, most of those experiments were done with AI research and training in mind (**not Large Language Model AI**, but general problem-solving AI research), and even if fun to play around with, they (understandably) are not suitable for game development, even if they could be used in gamedev.

The project that I have in mind **is very** gamedev related, though. The final product I have in mind is an online engine in the veins of [PuzzleScript](https://enginesdatabase.com/engine/puzzlescript/), in which someone could edit Tabletop game rules, and play them in the browser - and, if they find the game fun, they would be able to share and **play online with their friends**. This for me is the near-perfect tabletop prototype experience, being able to create the game in the browser and call your friends to play with you.

The online part I'm confident is going to be smooth sail because I have some experience with online multiplayer games. In particular, it seems to me easy enough because **as long as the players have the same tabletop game rules**, they only need to share their actions per turn, and the state of the board is deterministic. Not real time, and I'm not planning (for now) to create random components in the games such as card drawing and dices. The problem is deciding on what the Tabletop Game Engine is going to be made of, because I want something that is very iterable and easy to embed in other engines.

Enter Guile.

### why Guile?

I started looking into languages I could use to create such engine. The first thought that came to me was:

"Welp, I could just use Godot to create the whole thing and make an API on top of GDScript."

And that is probably the easy way out, but **GDScript is not an interpreted language.** I'd have to create a pseudo-language that would parse into the GDScript API, limiting much of what someone would be able to do on top of the Tabletop Engine app in real-time. I wanted to give users more freedom than that, but without having to create an interpreted language from scratch.

Also, using Godot for yet-another-project didn't look [Fun](https://dwarffortresswiki.org/index.php/DF2014:Losing) enough, so I decided to look into other options.

The Lisp family of languages picked my interest because:

- There are options to embed the language with C/C++ (e.g. [ECL](https://www.cliki.net/ECL)).
- Projects in Lisp seem to naturally develop their own DSL, which is very useful to create the tabletop language subset I'm imagining.
- I use Emacs for a couple of years but I still have no idea how to program in ELisp, so learning it would be very useful.

And of all languages from the Lisp language branch I looked into, Guile seemed the best fit, since ([from the website](https://www.gnu.org/software/guile/)) it is "designed to help programmers create flexible applications that can be extended by users or other programmers with plug-ins, modules, or scripts".

### the first prototype

To test how fitting Guile would be, I thought about pairing it with [Raylib](https://enginesdatabase.com/engine/raylib/), a 2D/3D game framework library written in pure C. The idea was to implement a little something in Raylib to render a simple window screen, and to draw in it using Guile on a REPL console.

A very important disclaimer first: **I've never programmed in Lisp/Scheme nor used Raylib before.** I was basically testing the waters to see how far I could go without much knowledge.

I decided that I would follow the [Guile logo tutorial](https://www.gnu.org/software/guile/docs/guile-tut/tutorial.html), but replacing GNUPlot with Raylib. I didn't expect much because I never seen a Raylib project before (although I've been following Raylib development for some years), but Raylib has a very good starting point on their repo with a lot of examples and a template with the Makefile ready - so I picked the "simple window screen", checked how I would be able to draw a line in the screen, and worked my way on the Guile integration. I picked the *Very Easy and Probably Wrong (TM)* solution of creating a separate thread to run the REPL Guile console from the Raylib application, so I would be able to connect to the console from my own computer. So, to summarize:

- A single `main.c` file initializing the window screen, creating a thread for the `Guile REPL` console, and creating the methods I would be able to call from Guile to draw stuff on the screen, Logo style.

![Screenshot of Logo Guile working](logo-guile.png)

It... kind of worked? The implementation is super simple, so I just posted the [main.c](https://gist.github.com/henriquelalves/8f6db4bc43b0e5161466ce972f7bf958) file on GitHub gist.

And surprisinly, the *Very Easy and Probably Wrong* solution of using a different thread for Guile didn't give me any headache. I just created the thread, started the REPL, and I could play with my own little Raylib Logo from an Emacs Guile console. The fact that the first solution that came to mind worked almost out-of-the-box on the integration are huge kudos on my book for both Guile and Raylib. This got me really excited on the kind of things that using Guile+Raylib combination enables me to do on my own projects, even beyond the Tabletop Game Engine.

My next step is to create a small [Game of Life](https://en.wikipedia.org/wiki/Conway's_Game_of_Life) simulation using Raylib and Guile.
