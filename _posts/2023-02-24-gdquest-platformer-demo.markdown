---
layout: post
title:  "Making Of: GDQuest 3D Platformer Demo"
categories: making-of gdquest
---

*This is a Making Of article of a Demo I made in Godot [@GDQuest](https://www.gdquest.com/). Most of the learnings are generic and can be applied anywhere.*

![Platformer Demo](/assets/posts/gdquest-platformer-demo/project-complete.png)

Our objective [@GDQuest](https://www.gdquest.com/) was to create a 3D Platformer demo in Godot, with the following requisites:

- Maximum of two weeks of development. We are creating demos fast, so we can have lots of content and test ideas using the latest version of Godot (at the time of the Demo, v4.0 beta 4).
- Inspired by *3D Mario games* mechanics.
- Make it simple and modular, so users can copy-paste the project into theirs or study the codebase.

I was the developer, and [Tibo](https://twitter.com/heytibo) was the artist behind this Demo.

## Setting tasks

Following the last TPS demo, the idea was to leverage nostalgia to create interest and making users more eager to explore the demo to their benefit.

The feature-set I decided upon to create a good Demo was:

- A character controller that would play like Mario 64 in the following ways:
  - Jump combo (triple jumping like in Mario 64).
  - Ground pound and ground related jumps.
  - Wall jump.
- Camera guidance similar to what Mario Odyssey.
- Simple button to ground pound and interact with the scenario.

Those tasks completely changed during the development process, but this will be better described below.

## Development and Decisions

#### Camera action

The camera movement was inspired by Mario Odyseey. My objective was to create a non-intrusive way to guide the player's camera, without affecting gameplay or where the player wants to look at. In Mario Odyssey, certain regions of the map slightly guide the player's camera to where the Game Designers want the player to pay attention to. This is a very light camera movement, and it stops if the player starts moving the camera on their own.

To replicate this behavior, I created "Camera Regions", 3D areas in the demo level that the camera would try following a predetermined path when the player is inside.

![Camera region](/assets/posts/gdquest-platformer-demo/camera-region.png)

The camera movement itself is created using two Paths:

- One path is the Player Path, or the closest point the player is from this specific line.
- The other is the Camera Path, which is the "optimal" camera position corresponding to the position the player is on the Player Path.

So if player is at the start of the Player Path (or the closest position in the Path from where the player is), the Camera will try moving to the position at the start of the Camera Path. When the player starts moving and their Player Path position starts changing, the camera position at the Camera Path will also change accordingly, and the Camera will try its best lerping to the new position.

One important thing was that even with a small velocity, guiding camera was really intrusive. So following more closely the Mario Odyssey behavior, when the player moves the camera, the "guiding" action resets and takes at least 10 seconds of player movement to go back to its previous behavior of following the Camera Path position.

Ideally, it would also take in consideration if the camera is more than X degrees from "ideal" position (don't animate camera when player has its own preferred perspective), but this ended up being a nice-to-have, yet to be implemented.

#### Arriving at the floor

When I started developing the Demo, jumping accross platforms was really difficult with a naive jump mechanic. After tweaking how the player was able to manipulate the character in-air (to better control air speed before falling), arriving at a platform got less difficult; but staying in a problem was really hard (even on bigger platforms).

The problem was the fact that the player, when arriving in a platform after a jump that needed precision, usually was tweaking the character in the air, so the analog position is in another direction. When touching the floor, the character would make a sudden move to another direction, making the player lose control of the chracter and falling from the platform if they arrived close to the edge.

The solution was to add frames in which the character is not able to change direction as soon as they arrive at the floor. If the character is pointing to another direction, instead of walking in the direction, consider the player as "braking" and then moving to the other direction (to give player time to tune direction they want to go).

#### Ledge grabbing

During the development of the game, I noticed that crouch related mechanics (ground pound / crouch jumping) was an added character state that would be much less benefitial to other developers using the Demo than Ledge Grabbing, which is a much more difficult mechanic to replicate and makes the gameplay much more interesting.

The initial mathematical idea for ledge grabbing seemed easy: raycast into solid objects (looking for ledges), and "glue" player character in the found position.

![Ledge grabbing wrong](/assets/posts/gdquest-platformer-demo/ledge-grabbing-moving-wrong.gif)

But this wouldn't work on moving platforms. Just gluing character in the found position wouldn't work, since the position could change in time depending on the platform the character was holding at. Deciding to fix this was important because this is a generic 3D Platformer solution for developers to use, so they should be able to animate any kind of 3D platform in Godot and copy-paste the Character Controller to play around with the controllers.

The solution was to save the global transform of the ledge's object, convert the ledge position (global position) into the object's position (local position), and making the inverse conversion each frame to get the new ledge position.

![Ledge grabbing right](/assets/posts/gdquest-platformer-demo/ledge-grabbing-moving-right.gif)

This solution worked most of the time, but moving platforms caused a lot of trouble with physics, and I had to tweak carefully when the player character should drop from the ledge (while holding the ledge, other platforms may push the player out of it, for example).

## Cutting corners

#### Crouch and stomping

Removed crouch/stomping as a mechanic. It just seemed to add a new state for the character that wasn't needed for the basic platforming action we had; instead, I focused on ledge grabbing, which is a harder mechanic to implement, and influences gameplay in a much more meaningful way.

#### Jump/Fall Gravities

When I was studying the reference, one of the design videos I saw mentioned that Mario had different gravities for jumping and falling (GameMaker's toolkit, didn't mentioned which 3D Mario). I decided to test and it made jump seem easier after a couple of iterations (having a bigger gravity when falling than when going up), but this was before tweaking other jumping variables (like the moment the player touches the floor after jumping).

Different gravity values stayed in the Demo until I decided to test on Mario 64 whether the "different gravities" was true. I recorded a video and counted the number of frames Mario stays in the air when jumping and falling; it's the same, so the previous reference was wrong (or at least not using the same 3D Mario reference as me).

After all the other tweaks, using the same gravity for jumping and falling made more sense. Thanks to the other tweaks, jumping seemed much less difficult and more natural. So I cut this specific feature out.

## Final result

![Complete demo](/assets/posts/gdquest-platformer-demo/complete-demo.gif)

At total, I spent around 10 business days in the core features of the Demo. It took me more time to tweak the Ledge Grabbing physics, since it could get finnicky with moving platforms, but I got really satisfied with the end result.
