+++
title =  "together devlog"
date = 2023-09-15
[taxonomies]
tags = ["devlog", "gdquest"]
+++


This post is something I planned writing since I finished making [Together](https://illusion-fisherman.itch.io/together), a game I created with my wife Brena Cardoso during the [Crossing Latitudes game jam](https://itch.io/jam/crossing-latitudes-jam/rate/2244216). The reason for this is that I was able to test some ideas with this game and consolidated my workflow publishing web games on itch.io, but for everyone wanting to do the same, it's worth to know some of the "gotchas" they might get.

![Screenshot of the game Together](together-2.png)

"Together" is an exploration and puzzle game, in which you have to find your path to climb a mountain. During the exploration, you collect letters you can use to write messages other players will see - but the message you write are always in a different language than the messages you'll read (Portuguese or Suomi). It's an asynchronous cooperation game, and we play around with the idea of the language barrier affecting the cooperation because that's one of the experiences me and Brena had having just moved to Helsinki from Brazil.

The idea of the game came naturally to us because when we heard about the jam, our minds were more-or-less settled on the idea of making a game that played with the language (since we just started taking Suomi classes). On the engineer side, I also wanted to use the jam to explore some topics:

1. **Making an asynchronous coop game.** I liked this idea because I had just finished a website project ([mastonews.com](https://mastonews.com)) and I finally had a good workflow making servers that was quick and I was confortable with - so I wanted to test this with an asynchronous multiplayer game.
2. **Making a pretty AND efficient 3D game on browser.** I'm used to create browser games with Godot 3.x for game jams, but I could never set on a workflow that allowed me to "prettify" 3D games confortably (the reason for that is that a browser game is much more limited in hardware capabilities than publishing the game as executables). I wanted to test baking lights (more on that later) to make the game look better, but I also wanted a workflow that allowed me to compress the game in the smallest binary I could.

So, let's dive in the results of each exploration a bit.

### basic asynchronous logic

I knew we were going to make a template system for the messages, similar to Dark Souls. So the server needs to store the messages each player sends with:

- The first message option chosen by the player (an integer).
- The second message option chosen by the player (an integer).
- The position of the message in the game world (three integers for a 3D position).

![Screenshot of Letter](letter-1.png)

This is only relevant to the client to parse the messages, the server actually just stores a string with the message content concatenated. For example, if a client posts a message on position (10, 15, 20) with options 1 and 2, the server will store the following string:

> "1;2;10.00;15.00;20.00"

When a client receives this message, it will parse it for the message options chosen and it's position, and it will spawn a "letter" object in that position.

How and when the players would receive messages followed a simple logic:

- When a player starts a game, I query for 20 random messages from the server and spawn the letter objects in their respective position.
- Everytime a player reads a letter, I delete the letter object and query for another random message from the server to replace it.

This solution have obvious (but minor) problems I chose to ignore (because Game Jam):

- A player may get repeated letters. I could solve this by adding an unique UUID for each letter in the database, and to query the database until I had a letter with a different UUID than the one I deleted or already spawned.
- Some regions of the game have less chance of spawning letters. Ideally, every region would spawn X amount of letters, because less players reach the end of the game than they start (so the starting regions have way more letters than the later ones).

![Screenshot of the Game](together-1.png)

### making an asynchronous coop game

My workflow is based on the [amazing tutorial by Sean Schertell](https://codepilotsf.medium.com/how-to-deploy-a-sveltekit-node-app-1c11171fe852) on how to deploy SvelteKit apps (I used it for Mastonews). This is a brilliant guide because it gives you the basic sysadmin workflow to setup a single and very simple server. You can use it for pretty much anything, not only SvelteKit! It also showed me `Caddy`, which for someone that head nightmares with `nginx` broken configurations, was the one thing that convinced me that "hey creating servers is not actually that difficult!".

I knew that for this particular game, I'd only need a server with an `SQLite` database to write player messages, and from where the players would query for messages to display on their game. This would be super simple to make if it wasn't for the one "itch.io" gotcha:

> Browser games in itch.io HAVE to use SSL for any request.

You don't need to understand how SSL works, but you already seen it: any (most) websites nowadays use "HTTPS://" - not "HTTP://". "HTTPS" is a site with SSL set and a certificate that guarantees the communication between client (e.g. a game) and the server is secure (as in, no one will be able to "eavesdrop" the communication). Itch.io understandably enforces SSL on browser games for security reasons, but that means you have to set SSL on your server if you want to make a game that communicates with your server. Setting up SSL means two things:

1. You have to buy a domain name.
2. You have to set your server to use a certificate from the domain you bought.

In my case, I've already bought "illusionfisherman.com" on GoDaddy. `Caddy` also automates the whole second step for me, so I literally only needed to follow the tutorial I mentioned setting up the domain name as "illusionfisherman.com" and that was it, server was ready to communicate with client.

The server itself is just a Node with `better-sqlite`, a JS package that is extremely straightforward and easy-to-use to deal with local `SQLite` databases. It's running on a `pm2` process, an awesome process manager that I also discovered following the tutorial I mentioned (Sean Schertell: if you ever read this, THANK YOU).

The second "gotcha" is setting up an encryption method for the messages between client and server, something the players themselves can't easily hack. I don't think this is something that would affect this game in particular, but I've seen players hacking leaderboards quite easily on other Game Jams, and I wanted to explore at least a simple encryption method between players and servers to avoid this kind of scenario in the future.

(A quick side-note: SSL is an encryption layer, but it's not helpful in the case of local players hacking the messages being sent to the server, because they are able to check and interfere with the message BEFORE it is encrypted by SSL)

I tested some things, but by far the easiest (to develop) solution I found was:

1. Create the message string to send to the server. Remember that the server only needs to store a single string for each message.
2. Calculate the message SHA256 hash with a secret string key appended at the end.
3. Send the hash and the message to the server.
4. The server receives the message content, appends the same secret string key at the end and calculate the SHA256 hash.
5. If the hash the server calculates is the same as the hash it received along with the content, this is a valid message.

A SHA256 string hash can be thought as a way to translate a string to an "unique" byte sequence. That means that the chance of two different strings to share the same hash is almost impossible (to the point of being irrelevant to be considered in the case of this game), and since the "secret string key" is not published anywhere but in the game's code and the website code, this is sufficient to "guarantee" that a message wasn't tempered with (unless the hacker knows the secret string key).

This is not a perfect solution, but it's a solution with a "good enough" hacking friction that makes the game at least not trivially hackable.

### making a pretty AND efficient 3D game on browser

The second challenge was to make a game that was pretty, but small and efficient to run in browser. I had a couple of ideas in mind:

#### using .glb 3D models, but make changes on import

This is something I learned working at GDQuest, but the ideal 3D workflow on Godot is to use `.glb` models with the least amount of changes (avoiding using inherited scenes or changing them in `.tscn` files). The reason for that is that when you inherit the `.glb` models and make changes to it in `.tscn` files, you are serializing the model (and the changes you made) to text format, which can make the exported binary of the game much bigger. This is better explained here: https://github.com/gdquest-demos/godot-4-3d-third-person-controller/pull/30.

Since me and my wife used some of the Kenney assets, the trick was to add a "Post import script" to the 3D models that would do the following:

- Reorganizes the model tree, so it could be converted to a MeshLibrary later.
- Creates a convex collision mesh and add it in the scene.

This also made my life easier whenever we changed some of the models - I only needed to change the model file and reimport it, since the only dependencies were scenes that were adding the ".glb" file directly into their node tree.

![Lightmap option](lightmap.png)

#### Baking the lightmaps

This wasn't the first time I tried baking a lightmap in Godot, but it was the first time the assets workflow made it easy enough to bake lightmaps. What I had to understand is that **reimporting assets with the settings you want is, most of the time, better than changing settings on an inherited scene**. This is also true for generating lightmap UV2 in Godot.

My workflow was to bake single regions of the game:

![Level scene](level.png)

And have a "world" scene that would control which level the player was in, and show/hide levels accordingly:

![World scene](world.png)

This is a similar workflow I had for a [metroidvania game I made for Metroidvania Month 17](https://illusion-fisherman.itch.io/would-you-still-love-me-if-i-was-a-banana-peel). Having the entire level already instantiated is not optimal but the level is small enough to not make a difference - but having the entire world visible to you is REALLY helpful in designing the game.

### Summing up my experience

I feel I was a bit greedy at the start of the jam, because me and my wife had Boring Stuff (tm) to do during the jam time, so we'd have less time than we wanted to make the game. But things got aligned surprisingly quickly, and the engineer workflow I had in mind worked really well. Brena is also amazing at adapting the assets and animations we found, and creating new assets ad-hoc.

The end result is a game that works really well in the browser, smaller than most games I made before (some of them even simpler than "Together"), but packing quite a bit of development love from me and Brena.

![Ending](ending.png)
