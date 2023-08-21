+++
title = "Making Of: GDQuest TPS Demo"
date = 2022-11-30
[taxonomies]
tags = ["making-of", "gdquest"]
+++

*This is a Making Of article of a Demo I made in Godot [@GDQuest](https://www.gdquest.com/). Most of the learnings are generic and can be applied anywhere.*

![TPS Demo](/posts/gdquest-tps-demo/project-complete.png)

Our objective [@GDQuest](https://www.gdquest.com/) was to create a Third-Person Shooter demo in Godot, with the following requisites:

- Close to one week of development. We are creating demos fast, so we can have lots of content and test ideas using the latest version of Godot (at the time of the Demo, v4.0 beta 4).
- Inspired by *Ratchet & Clank* mechanics.
- Make it simple and modular, so users can copy-paste the project into theirs or study the codebase.

I was the developer, and [Tibo](https://twitter.com/heytibo) was the artist behind this Demo.

## Setting tasks

Players will always compare Demos or Prototypes to the closest game from the same genre and with similar aesthetics, so playing with the affective memory of users has the upside of making the Demo feel better to play, leading to more invested time by each user and a deeper analysis on the mechanics. It does have the downside of increasing the expectations users have on the Demo and possibly making it feel "worse" to play for the lack of content, but having an exhaustive feedback that you must filter is better than not having enough feedback.

So the feature-set I decided upon to create a good Demo was:

- A good character controller, with camera action similar to *Ratchet & Clank*.
- Melee attack combo.
- Two weapons: a gun and a grenade.
- Two kinds of enemies: a standing one that shoot bullets, and a walking one that follows the player.
- A collectible like *Ratchet & Clank* metal bolts.

Those features would be basic elements to create an "intro" level to a *Ratchet & Clank*-like game. No element alone needs an in-depth explanation, and the user can pretty much test the demo without reading any instruction to understand the references and play around the level like a playground.

## Development and Decisions

#### Camera action

One thing I noticed working in the game's camera control is that games like *Ratchet & Clank* are polished for levels with distinct 'arena' spaces, with more horizontal action than vertical exploration, and camera is close enough so it won't move too much when you are aiming from the shoulder.

![Ratchet & Clank screenshot](/posts/gdquest-tps-demo/ratchet-screenshot.jpg)
*Ratchet & Clank (PS2)*

My first thought was to create the usual Camera pivot following the character, but *Ratchet & Clank* Camera always follow the character's ground height, not the Character itself. This feels much better to play, as you can jump around to avoid enemies and enemy projectiles without affecting your current view, so I created this behavior with a "Camera Controller" object with `top_level` toggled (a nifty Godot v4.0 `Node3D` toggle that allows the object to have a transform that doesn't inherit it's parent node). The Player's ground height is updated every frame using a `Shapecast` node (it's basically a `Raycast` but using a cillinder shape, so the ground height is correct even when the player is at the edge of a platform).

#### Creating the grenade path

The grenade weapon itself is quite simple: it's just an object that, when thrown, checks for enemies in an area around them to damage. The problem was creating a good grenade trajectory visually and the look-and-feel of the grenade throw action.

My first approach was to create a `Curve3D` (in a `Path3D` Node) which the grenade would follow using a `PathFollow` node. The `Curve3D` had some default points parameters, and I would only change the start and end points of the curve to generate a new grenade trajectory.

Instead of generating an ArrayMesh procedurally to make the visual path, I used a `CSGPolygon` with the `path` property set to the `Path3D` Node (CSG is not ideal when changing the geometry in runtime, but in this case it was basically an easy-win). The result was okayish, but the grenade felt weird when thrown because it was following the curve linearly (both horizontally and vertically), and not in a ballistic trajectory (linear horizontal movement, but quadratic vertical speed).

![Grenade first approach](/posts/gdquest-tps-demo/grenade-first-approach.gif)

The second approach was to calculate the Grenade trajectory procedurally using ballistic equations, and feed the new points into the `Curve3D`. I'd still use the `CSGPolygon` to generate the curve visually, but instead of using the `PathFollow` node, I'd use the ballistic equations to know the velocity I'd have to throw the grenade, and I'd trust Godot physics simulation to do the rest. This made the ballistic trajectory feel a lot better compared to the previous attempt, but for some reason that I couldn't figure out, the trajectory was always a little bit different than the calculations (not enough to prove the calculations wrong, but different enough to make some grenade trajectories miss the target by a really small margin).

The final approach became an "evolution" of the second approach, adding a small correction each step during the grenade trajectory to make it stay close to the curve the player expected to see (forcing it to hit the target regardless), but mostly trusting Godot physics for the look-and-feel of the trajectory. The physics approach also made it possible to add a little "bounce" effect: the grenade triggers when it collides with the environment, but it explodes a couple of milliseconds after.

![Grenade third approach](/posts/gdquest-tps-demo/grenade-third-approach.gif)

#### Blocking the Level with CSG Mesh

I got inspired by the first couple of minutes of the first `Ratchet & Clank` level, in particular the simple level jump setup. So I used Godot CSG Mesh extensively to block the level and test some platforms.

In the time I was developing the Demo (Godot v4.0 beta 4), CSG Mesh had some problems:

1. On subtraction operations, Godot would sometimes crash. I'm still not 100% sure, but most of the time Godot seemed like it crashed because the meshes in the CSGMesh operation had conflicting vertices (vertices or surfaces were perfectly aligned).

2. The result mesh had disappearing surfaces.

3. The result mesh collider had "invisible bumps" that would affect the player movement on the surface.

Problems 1) and 2) were solved by tweaking the CSG primitive sizes in 0.001 increments, so they wouldn't have conflicting vertices nor disappearing faces in the result mesh. Problem 3) was solved by changing the Player collider: instead of using a single Capsule shape for the Player collider, I added a small separation ray at the bottom, to force the Player physics object to ignore eventual "floor bumps" without affecting the Player's collision with the overall scenario.

## Cutting corners

#### Melee Attack Combo

I thought I'd be able to create a melee attack combo flow for the character controller, but I noticed a single attack with a light impulse was enough to make the demo interesting, since combat wasn't in-depth and I felt users would mostly use the weapons.

The lack of "depthness" in the combat may be caused by the the fact that *Ratchet & Clank* combat has light resource management mechanics, which wasn't the focus on this Demo but does create an interesting choice for the player (use weapons and waste ammo vs store ammo for the hard parts of the level). Since we wanted users to test the Camera and changing weapons, it didn't look like it would make sense to limit the amount of ammo players had.

## Final result

At total, I spent 5 business days in the core features of the Demo, plus some days to tweak mechanics (mostly the grenade throwing) and integrate the beautiful art assets made by [Tibo](https://twitter.com/heytibo).

![Complete project](/posts/gdquest-tps-demo/complete-project.gif)
