---
title: How I taught Phaser to beginners.
lead: A report on my Phaser Workshops, as of 2017.
---

This post is a simple report on how I gave both of my Phaser workshops last month (Oct 2017) and what was my approach. My intention is not to preach the "right" way to do it,  but rather just document my experiences and suggest what I think is important to teach about Phaser to beginners on the Framework. The workshops had the objective to teach the basics about Phaser, and to spread the word about the Livre Game Jam event, a game jam I was helping organizing in my University (Unicamp) in which we would help the Jammers on using only Libre Software to create their games, and publishing them using GPL. The final jam results can be found here: .

The complete material of the workshop can be found here (in portuguese): https://github.com/henriquelalves/Phaser-Workshop-06-10-17. I will reference the slides on this report.

For the workshops, I used a simple Phaser boilerplate that can be easily run on any Linux/Mac (on Windows you got to have a HTTPServer software to do the job for you instead of python) (in english): https://github.com/henriquelalves/Basic-Phaser-Boilerplate. 

This post will be very extense because I'll be detailing more what I talked about during the workshops, but bear in mind they were only 2h long; the whole introduction until (but not including) the "Hands-On" section usually took me 30 minutes at most, and the rest was used for the "Hands-On" (with a rushed Conclusion at the end).

### Background

You can check my complete bio at my website; currently, I'm working as a Game Developer at Clickideia, a digital educational portal for schools in Brazil. When I just started working there, I had the freedom of choosing which tool I would use to develop the company games; the only limitation would be that they had to run in Browsers (and possibly in very mediocre computers, since we are talking about public schools in Brazil). I had some background in Libre Game Engines/Frameworks, so I knew about Phaser existence and recognized that it could be very useful to the job - as of now, Godot (2.1 brach) has a buggy HTML5 exporter, and Game Maker / Unity produce HTML5 games with a quite heavy engine overhead (plus, they are not Free Software). So I started fiddling with Phaser, and after launching some games on the portal and on mobile, I can say with some confidence that not only it was the right tool for the job, it is a great framework that is easy to learn **if you are careful around Javascript** (more on that later).

The following sections are in the exact same order of explanation of my workshops.

### What is Phaser
*Slides 5 - 6*

Phaser is a **Javascript Framework** for the **web**. I'll explain each keyword separately in my own particular order of importance like I did during the workshops.

**Framework**: A *framework* is a programming library that can assist in a specific programming task. It is important to differentiate a game development *framework* from an *engine*, as (usually) they have different scopes. A *framework* is the support structure in which the application logic is built upon, and it has the core elements and tools you are going to use; you'll have to extend them to your like so you can develop your software. An *engine*, on the other hand, is the "complete package"; it will feature all the elements that intersects what you want to develop to what the OS will show to the user. Generally, this means that by using a *framework* you'll have more freedom on how you are going to build your game, but you'll have to work more on glueing together the pieces of structures other than what the framework has to offer, and you'll be a lot more in contact with coding than with graphical interfaces. In the case of Phaser, it will make our life easier by using the Web Browser as its "Game Engine" (the browser will handle all the graphics and sounds for us, so we won't touch the OS calls and stuff).

**Javascript**: Now this is what I consider Phaser's most dangerous "advantage". Javascript is a very beginner's friendly language; it is very permissive on what you can do with it, every platform has access to it (just open your browser's console), and it is packaged with a lot of features thanks to HTML5. Thing is, Javascript is what we call in Brazil a "várzea" language; it can seduce beginner programmers in not protecting variables from the incredibly dangerous global scope; the "undefined" static type can have some pretty wild behaviors where you least expect it; and closures are somewhat of a hard concept to grasp when you are just starting on the language. By using TypeScript you correct some of the Javascript wild behaviors (TypeScript is a Javascript "superset", which means it acts like a language on its own, but underneath it is just Javascript), but unless you get through the setup hassle, I think it's just easier to learn how to use Javascript in a safe manner. Also, ECMA 6 (the 6th version of Javascript) has the "class" keyword support, which make coding more recognizable - unfortunately native ECMA 6 won't work on older browsers, so keep that in mind.

**Web**: Althought pretty obvious at this point, it is important to emphasize how the web environment affects Phaser. First, Phaser can't be tested 100% offline; in layman terms, you have to at least "pretend" you are online so Phaser can work on your local computer (that meaning you have to create a local server to play your game). This happens because the browser isn't allowed to read your local files (if it could, we would have a lot more security problems to handle); it can read from a server though, and that's how Phaser load your assets. If you have python, testing phaser is as simple as typing in your terminal "python -m SimpleHTTPServer 8000" and in your browser typing the address "localhost:8000", but it is important to understand why such setup is necessary. Second, as said earlier, the browser itself is Phaser's Engine; it will handle the OS calls to draw on the screen, play sounds, render fonts, etc. Phaser will try its best to make your game behave normally in all browsers, but sometimes you'll have to remember that your final user may have a different browser than what you are testing your game with - so be careful, specially about sound formats and such.

For the workshop, I chose to create a simple Phaser Boilerplate project without any dependencies and using ECMA 6; I did that because I wanted to teach how to approach Phaser modularly (eg. creating each game state in a different script), and "Class" definition is something most programmers know about thanks to OO paradigm. Also, I commented the important bits of the boilerplate as a guide for whoever fancied the code.

### Phaser important features
*Slides 7 - 12*

Those are what I consider important features to understand before trying to tackle Phaser in a self-taught manner. In no way this list is definite or sufficient; it is just what I consider the basic bits of Phaser that, if well understood, can make coding much easier.

**Game States**: Game States are what your "game scenes" look like, which means you can "divide" your game in particular pieces that are somewhat independent from each other. For example, a simple Atari game would have a "Main Menu" screen (usually the game title in big letters and waiting for the first player input), the "Game" scene, and finally a "Final Score"/"Game Over" screen. It is no coincidence that there is a "Game State" class in Phaser we can use, and understanding the Game State flow on your game is important because as I said, they are independent, so we'll have to override some Phaser methods to get them working alone. The important methods, in the order they are called by Phaser when you "enter" the scene, are:

--> Preload: It's here where you are going to load the assets you are going to use on this Game State. During my workshop, a question that appeared was "why I would load the assets before, can't I just call the assets I'm going to use on the fly?". This wouldn't be good, because Browsers like to load everything they are going to use BEFORE using them. In any kind of game loading stuff on the fly is usually a bad idea because it will slow down the framerate pretty heavily, hence a method just to load everything before using the assets, from images to sounds and fonts.

--> Create: After loading your assets you can create your Game Objects - which are basically the game elements with a Phaser Logic added to them. For example, you could draw an image on-screen without Phaser help; but than you wouldn't be able to manipulate such image or make it interact with other Phaser elements, unless you create the whole logic for it yourself (which beats the very reason we are making Phaser doing work for us efficiently!).

--> Update: This is the main Game Loop (this method will run each frame) and it is where most of your game logic will be.

**Game Objects**: As explained earlier, Game Objects are any assets or game elements with added Phaser Logic behind to give you ways to manipulate them. It can be anything from Sprites (images that can be scaled, animated, repositioned, etc), Sounds, Texts, Shaders, and a whole lot more. By extending one of Phaser Classes, you can easily create your own referenciable Game Elements, modularizing your code as you see fit.

**Tweening**: "Tweening" is basically giving the computer the first and the last state of a Game Object, and telling it to play the animation that happens *between* (eg. if you give a start and ending position of an image, it will move for the duration you specified). Now, the real reason I find this particularly important is because I worked this last year on Educational Games; they have to be particularly polished on the animation side, or else children will lose interest on them very quickly. Phaser tweening is amazingly simple, and the fact that you can animate pretty much every element on Phaser makes it VERY useful for simple animations without adding unnecessary logic.

**Examples**: Internet is full of Phaser examples; a particularly useful site is https://phaser.io/examples - the simple stuff is all there with easy to understand examples, and I really recommend looking for examples on the mechanics you want to add to your game before tackling the challenge alone. Also, documentation for Phaser is very complete (https://phaser.io/docs/2.6.2/index) and easy to understand after you understand javascript itself.

### Hands-On
*Slides 13 - 14*

This post intention is not to be a tutorial but rather a suggestion on how a Phaser workshop CAN be; therefore I won't explain how to create a Phaser game bit-by-bit here. Instead, I'll explain the logic and the order of explanation for the game we created during the workshops.

#### The game idea

I chose the game based on interesting mechanics and simplicity; at first I thought it would be nice to create a "platform game" example, but that's literally the first thing you learn if you start from Phaser.Io website. So I went a little back and remembered the old Doodle Jumpers that had quite some success (and clones) some years ago. Instead of a Doodle Jumper, I went with a "Doodle Faller" (you have to keep moving between platforms that goes up the screen without getting offscreen for the longest time possible). This would allow me to create the game logic from zero, hopefully making the architecture more understandable and allowing a little bit more freedom to be playful with the game design.

#### Starting the project

First thing was to set up the project; fortunately it went according to what I planned, as in the workshops everyone just downloaded my git repository Phaser Basic Boilerplate (by .zip or just `git cloning` it) and we used Linux computers, so I was sure that everyone would be able to create the a simple HTTP server using python. This allowed me to focus more on explaining how the blank project worked, from the simple HTML script calls to each individual script.

A quick sketch of the project organization:

--> `index.html`: Loads `libs/phaser.min.js` (the framework) and all js scripts we were going to use on `/src`.\\
--> `assets/`: Where all the game assets are (in the workshop case, just the player image, background and platforms).\\
--> `libs/`: Where the 'static' code is; in the case of the workshop, just the Phaser script.\\
--> `src/`: Where all the game javascript code is.\\
|--> `src/main.js`: Setup the main Phaser.Game element; creates the game states and instantiate them in our Game; than, start the game.\\
|--> `src/states/`: Where the Game States code is.\\
||--> `src/states/menu.js`: The game menu.\\
||--> `src/states/game.js`: The main game state.\\

The only problem was that, since I used ECMA 6 without BabelJS, each script would have to be loaded on `index.html` before being called on `main.js`. More advance users should look for NPM phaser boilerplates with BabelJS funcionalities.

#### Menu improvement

To start the code Hands-On, I suggested we should improve the menu by choosing a name for our game and creating a Title screen (by using the Text game element). Than, to make it fancier, we would tween the title to appear on the screen by bouncing or just flowing in.

#### Player creation

The first game bit we created was the player; it worked well explaining how to load the player asset (the difference between loading as a spritesheet and as a single image), than using it to create the player sprite and using the `update` method to create the keyboard input logic and movement. Finally, the last thing to add was the gravity logic, as this would make the player fall through the screen and making us having to create the platforms where the player could stand.

#### Platforms spawning

Before creating the collision logic, since we were doing everything by hand, I explained how we should approach the spawning of the platforms. First, we created a single platform on-screen and updated its y position on `update`; than, I explained how to use a Phaser `timer`, and we created a method do spawn more platforms on an array; finally, we looped through the array on the `update` so that all of the platforms y positions would be updated.

#### Collision logic

Finally, to finish the main gameplay code, I explained how we could create the collision logic by checking on update when the Player is "inside" one of the platforms, and updating his position and y velocity accordingly. I chose creating collision logic with a simple point position test because we would be able to create the entire logic from scratch and on the `update` method, and by using collision boxes we would have to create added logic for some movement tweeks that would happen (colliding with a platform from the side, for example).

### Conclusion
*Slides 15 - 19*

Finally, the final slides are just a quick thought exercise on what the game is still missing; stuff like *screen management* (so the game can be playable on any screen), a *preloader state* (by loading every asset on one single state, so state transition can be quicker and safer), making the game *mobile friendly* (using touch events instead of keyboards), and stuff like *state transition animations* and other quick *polishments* we could make.

### Workshop feedback

For both workshops I created a small anonymous feedback forms; the results can be found [here](https://docs.google.com/forms/d/1g39aPMpgGfVqNjx2T7zxHmiQgnAs5-he11eabrMyV-4/viewanalytics) (workshop in Facamp) and [here](https://docs.google.com/forms/d/1tZjEAPC5hvYFTMbDo7pe7pffXpWsL8dR36c1ooNfCd8/viewanalytics) (workshop in Unicamp).

Interesting stuff I want to point out:

**Logic vs Syntax**: I often said on both workshops how learning syntax was less important than the logic itself; syntax is something you can only learn by practicing it, but understanding the logic of the game architecture we were creating would make becoming self-taught much easier after the workshop. I feel this helped Javascript beginners feel more confortable during the workshops, as I often paused during the Hands-On moment to help around with the progamming problems often happenned, and even if someone was just copying the code, they tried to understand what was being copied.

**Explanation line-by-line**: Overall I had a good feedback on my code explanation because I sometimes paused the coding and explained what was being written as if it was being phrased. As we were using classes, coding becomes a lot of `this.[...]` to protect variables from global scope, and after some dots people can get confused pretty quickly on what is being called by who. Even if this took me some more time, phrasing out some logic pieces helped people getting into the same mental frame of the game architecture.

**Time management**: I had trouble scoping out what I could do within 1h30min of Hands-On experience with Javascript, but in the end we could complete a Doodle Faller - somewhat. It still missed a Score system and a Score game state (creating a Score Game State and inserting it into our architecture was part of my plan). Unfortunately it wasn't possible to finish it on the first workshop, but the overall workshop flow was good (we finished the main game logic, which was the Hands-On objective), so I kept this flow to my second Workshop.
