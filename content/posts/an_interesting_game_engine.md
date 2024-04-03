+++
title =  "free software and game development"
date = 2024-04-03
[taxonomies]
tags = ["gamedev","essay"]
+++

This post is me putting down some thoughts about Game Development, and specially how an indie game developer might succeed in today's industry leveraging Free Software.

A very important disclaimer about me: I'm a Free Software enthusiast, but I'm okay doing compromises for game development. But ever since I started understanding the Free Software movement, I've been wondering: how could I leverage the benefits of Free Software for game development?

This topic is something I've been rumminating for a while from different angles, but I think the main question that drove me to a "sort-of" idea was:

### how can indies sustain themselves in this hellscape of a market?

I'm currently working in the Mobile gaming industry (and have been for a couple of years), but I hold the indie dream close to my heart (as most game developers do to stay sane), so this is a question that bothered me a lot, specially after so many layoffs I've seen in those last 2 years. This is a very difficult question to answer, but I think there are two challenges that are the biggest threats to most indies:

- **Distribution**. Digital markets are saturated with new content, even Nintendo that a decade ago had the highest-walled garden of them all is now victim of a rampant number of shovelware games in their marketplace, making new games visibility a lot more difficult. 
- **Financial risk management**. Triple-A figured out the solution for this challenge was live-service games (or GaaS), [but it seems like this is not going well for Triple-A either](https://www.thegamer.com/suicide-squad-season-1-joker-dlc-addition-grindy/). Indies have it worse, since content development over-time is a very hard problem to tackle, so developers are always 1 "failed" game of going bankrupt.

The usual answer for the first challenge is to **find a niche** a game can get some spotlight, even a small one but with guaranteed organic growth. The answer for the second challenge is diverse and depending a lot on the medium - mobile games from small teams can add small doses of live-service or stick with ads and a casual audience. Games on other platforms might use Kickstarter or look for a publisher to offset financial risks before the game starts development, but usually that means that there is a portion of development that you're just praying for the best until the game is in a state you can pitch for.

The big picture is that making a plain-old "premium" game with a small team is a **difficult** endeavour with a low margin of financial success, specially if you can't self-publish your game. But are there alternatives that haven't been tried yet?

### first, let's talk about live-service games

Live-services games are getting a lot of heat as of recent, with [reported player burnout from all live-service games out there](https://www.pcgamer.com/im-officially-exhausted-by-all-the-live-service-games-and-i-want-to-see-way-less-next-year/) to [live-service games shutting down because of the lack of player interest](https://www.windowscentral.com/gaming/marvels-avengers-is-shutting-down-to-be-delisted-later-this-year). Even then, the financial risk makes so much more sense from a corporate perspective that [big publishers are still doubling down on it](https://gamerant.com/warner-bros-focus-shift-live-service-games/).

And it does make sense. Working on Mobile gaming on those last couple of years made me realize that the alternative is betting on a single big-hit of a game - and the meaning of a "big-hit" scales dramatically bad considering how expensive making a game is. The distribution problem means that UA (User Acquisition) costs are sky-high and only getting worse, and organic growth on the mobile space depends on hitting a magic SEO formulae that might change with the phases of the moon, or reaching Top Grossing/Top Downloads levels in the mobile stores, driving UA costs even more. To my knowledge mobile stores are the first to reach those levels of saturation, but other digital fronts are not that far behind anymore. So making a live-service game means that you're betting on a player pool that is sticking to your game for a longer time, possibly paving the way for whales to spend A Lot of Money in the game, and thus justifying the costs.

Live-service is much more difficult for indie developers with limited resources, because:

- It depends on having a solid player-base from the get-go or to have enough money to acquire said user-base. This is simply not the case most of the time.
- It depends on more content being developed over time. Even with a solid user-base, it is very hard for small developers to fullfil the expectations of more content with the cadence needed so their game stay relevant.

Worth mentioning that live-service games are not inherently "evil". We've seen live-service games with fair models for IAP's, and even single-player games with "small doses" of live-service in their multiplayer mode counterpart. But this is strategy for lessening financial risks is only feasible to triple-A and huge indie hits.

An important idea associated with live-service games is the concept of "lifeness" of a game. A "Live-service game" means that there is a service coupled with the game that is bringing a constant flow of new content. "Live" here is associated with the online nature of the service, but the **lifeness** of a game is inherently associated with this constant flow of **new content** - a game that is **alive** is a game that has a user base still playing it, but more importantly, that is still generating content. The content ranges from community challenges such as speed-runs (e.g. the Mario 64 community is [very much alive](https://www.speedrun.com/sm64) to this day) to user created mods (e.g. the Elder Scrolls IV: Oblivion community is [very much alive](https://www.nexusmods.com/oblivion?tab=new+this+week) too). What is interesting here is:

- A **Living Game** can be self-sufficient, as long as there are players willing to engage in the game more profoundly and to share it with a community.
- A **Living Game** attract new players organically because players cheerfully share the things they create, and any content that bursts the game community bubble is a success in the players own eyes.

**Living Games** are very clearly enablers of user generated content (even Mario 64 has a very rich scene of modded roms, thanks to its decompiled source code), and can reap some of the benefits of a live-service game and its financial risk mitigation, but without the development costs. That's my main argument on why this is interesting for indie developers.

There is a counter-argument that reaching the status of a **Living Game** is a random shot from the already difficult task of making a mildly successful game, but I think we've been tackling this problem the wrong way:

1. A game becoming a **Living Game** may come with an absurd amount of success, but many successful games have very different "degrees" of lifeness after their initial success, so there is a case of strategies on how to enable a **Living Game** better than others. 
2. The tech matters a lot. How easy and accessible is for the player to understand the tech matters a lot. The usual solution is to create special tools for the player to create content (e.g. Mario Maker 2, Dreams by Media Molecule), but I believe we could go a step further **without** having to maintain special and costly tools.
3. The game doesn't need to be a major success at launch as long as it enables easy content generation. In the long-term, it will have better chances of attracting a small user base from niche creative communities, which can drive the long term organic growth of the game.

I don't think a successful indie game made to enable user generated content this way can easily reach the sky-high profits of a successful live-service game, but I firmly believe that a game made to be a **Living Game** from it's conception is the Indie Developer equivalent of mitigating risks with long-term player content generation.

Which finally brings us to:

### an engine for living games

[David Thompson](https://dthompson.us/) has a very interesting quote on his [itch.io page](https://davexunit.itch.io/):

> I try to make tools that advance the REPL-driven development methodology, resulting in games where the runtime and editor are the same program.

David is the creator of [Chickadee](https://enginesdatabase.com/engine/chickadee/), a Guile framework for game development (which I do recommend taking a look if you are interested in Guile/Scheme for game development). I think a game in which *the runtime and the editor are the same program* is the ultimate goal of a game engine that enables a game to become **alive** easily - the game engine itself is the game, and advancing the tools to create the game also advances the tools available to users to change the game. The idea of a **Game Engine for Living Games** is to naturally transform game-development effort into tools for user generated content, but without locking the design of the game into a specific set of tools. The game and the editor are already one and the same, so the game developer can better tweak the balance of development effort and making the learning curve of the tools less steep, for himself and for the players. This also means there is only a thin line separating iteration and play - a Game Designer should be able to iterate on a level while it is played, and so should players.

An analogy for a game made with this Game Engine would be to give a player an already built Lego car. How fun the game is depends on how fun playing with this Lego car is. But the player can also disassemble the car and reassemble it any other way, and the Game should not only allow that but be built with with this in mind. Comparing it to other games:

- Mario 64 is a great game, but it wasn't made to be disassembled. It's like a great toy to play with that the community create their own tools and guides to disassemble the toy and create what they want.
- Elder Scrolls IV is moddable, but not frictionless. There is a big line separating a player from a content creator. It would be like a toy that can be assembled with different components, but each individual component is a black-box piece much more difficult to take apart.
- Little Big Planet is much more fine-grained and player-friendly than Elder Scrolls, but a player doesn't have freedom in how to mod or redistribute the changes in their game but to use the live-service portion of it. The analogy falls a bit apart here because Little Big Planet is one very closed ecosystem but with very open-ended and friendly tools to create user content, but the current state of the game is an example of how its "liveness" wasn't made to outlive its live-service: players that like Little Big Planet 1 or 2 can only play the multiplayer via pirate servers on homebrewed hardware. As an indie, we want to build a **Living Game** free from the necessity of also maintaing a live service.

The perfect **Game Engine for Living Games** is a Game Engine that run as editor and runtime, and has user freedom to modify and redistribute the game deeply rooted in the way someone develop a game in it and on it. 

This finally brings us back to the [4 freedoms of Free Software](https://www.gnu.org/philosophy/free-sw.html.en):
 
>- The freedom to run the program as you wish, for any purpose (freedom 0).
>- The freedom to study how the program works, and change it so it does your computing as you wish (freedom 1). Access to the source code is a precondition for this.
>- The freedom to redistribute copies so you can help others (freedom 2).
>- The freedom to distribute copies of your modified versions to others (freedom 3). By doing this you can give the whole community a chance to benefit from your changes. Access to the source code is a precondition for this.

Those freedoms by themselves won't make a game commercially successful, as in making the game GPL won't make it magically a good "Living Game" example. But a good **Free Software** has a similar set of characteristics than a good **Living Game enabler** Game Engine:

- It should be **easy** to run a program in any way you want for any purpose / It should be **easy** to run a game with no live-service blockers.
- It should **easy** to study a program source-code and to change it with immediate results / It should be **easy** to create mods for a game and/or change its core behaviors (think the **editor & runtime parity** discussed earlier).
- There should be **no blockers** for redistributing a software with the modifications  / There should be **no blockers** for users redistributing the modifications they made for a game.

This is my argument that an intersection of interests exists between Game Development and Free Software. How this can be of better use for game developers and what this Game Engine would look like is an educated guess because I don't think we have seen a prime example of that - at least not yet.

### speculations on a "living games" game engine

For any game created on this Game Engine, it should be **dead easy** to modify the runtime. The mission of this game engine should be to **allow users to modify the game while it runs**, and have **development tools available at user request**.

Remember: **the runtime and the editor are the same program**. Let's imagine we're creating a platformer game. As game developers, we might create tools to edit levels, place enemies, modify physics settings, etc. The core game experience **shouldn't** show those tools to users - after all, we are focusing on creating a good game experience that is independent of user generated content at first. But we want to enable this game to become a **Living Game** - so at user request (e.g. a "Development Mode" toggle on the settings menu), users should have available for themselves the tools we used to create and edit levels.

**Engine code** and **Player code** should be different and clearly defined:
- **Engine code** is mostly agnostic code to run the game optimally. All things non-game logic that support the Game (e.g. Rendering, Input, etc) should exist in this space.
- **Player code** should exist and be modifiable in runtime, and is all game-related logic. Editor tools are **Player code** - the game developer is the player and any player can become a game developer.

All **Player code** modifications should be compatible with the **Engine code** that supports it. Modifying **Engine code** shouldn't need to be as easy as changing **Player code** - it's a compromise to differentiate code that needs to be optimized from code that needs to be easy to modify.

User modifications should be easy to reverse. Players should be able to get to the original state the game was distributed in, without being forced to backup their games to not lose the original state.

About licensing: I believe GPL should be a flag for games that have in their core concept the idea of becoming **good Living Games**. There are great open-source games out there with permissive licenses, but I think we could leverage what GPL means to better classify games that were made to be **prime examples** of software that respect user freedom **and** make user freedom **easier to exercise**. Any **good game** can be open-source, but a game can only be a **good open-sourced good game** if they were made with this objective in mind from conception. GPL should be the license for this kind of game.

### some final thoughts

I've been meaning to put those thoughts down for quite some time mostly because I think we've been missing some interesting games that could have been made, and interesting ways of making games other than using the [many game engines available out there](https://enginesdatabase.com). In parts, I think this essay depends on the wishful thought that **we haven't seen a big commercially successful Free Software game because no well designed game have went all-in on software freedom yet**. I might be wrong - maybe I don't know the prime example of successful Free Software game that already exists, or worse, maybe there are good games that went all-in on software freedom and simply haven't achieved success, despite my arguments on how it could mitigate risks.

I do like the concept of **Living Games** I described here, and I want to explore the possibility space of creating good living games in the (near) future.
