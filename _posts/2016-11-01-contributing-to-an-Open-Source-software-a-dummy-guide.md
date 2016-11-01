---
title: Contributing to an Open-Source software, a dummy guide.
lead: Or, how I tried helping Godot game engine with GitHub pull-requests during Hacktoberfest 2016.
---

My intention in this post is to help those that are insecure about how to start contributing to Open-Source softwares, so the tips I'm about to give covers from choosing a software to contribute for to the practical methods I found worked best for me when I started tackling the software issues. Hopefully I'll spark some motivation or even interest in contributing to FLOSS software (FLOSS englobes a [lot more ground](https://www.gnu.org/philosophy/open-source-misses-the-point.en.html) than Open-Source), but I'll try to stick with the experiences I had and generalize the tips from there.

This post was written during [Hacktoberfest 2016](https://hacktoberfest.digitalocean.com/) - the event itself was my motivation to start working on back-end contributions to one of my favourite projects, so you can also count this post as a little journal of the things I noticed and avaliated during the whole process. I'm not a professional myself, so any feedback is more than welcome!

## First step: Choosing a quest

This first step looks obvious, and have an equally obvious explanation: you should be careful choosing the software you want to contribute with, because you'll have to tackle some of its problems as a hard working programmer. If you don't like the job, you'll feel like you are just working as a programmer for free; which is basically true (not much of a plot twist there, sorry). The thing is, cliché as it is, working for free in a software that you CARE about is not just working, is creating something meaningful - maybe learning new stuff, maybe just improving the software that you and a community enjoy using - and that will be the source of your motivation during the whole process.

Now I could spend some good lines on how FLOSS software makes all the difference here, but there is already a lot of material on the web about it. The only thing that I want to highlight is, because of the very nature of a FLOSS software, every bit of contribution you create is meaningful, and this is one of the greatest sources of motivation. Also, it is very important to have a community to back you up when you don't exactly know how to work with the back-end of a software - and there is hardly any more developer-friendly community than the FLOSS world. So my recommendation: work with a software that you use, has a permissive license, and has a community that you can make easy contact with.

In my case, I chose [Godot](https://github.com/godotengine/godot/) precisely because of that; it's a Game Engine that I use a lot, has a permissive license (MIT), and has a very good community that covers a lot of internet ground (Forum, Facebook, IRC, Reddit, etc.). But there is a LOT of other interesting softwares out there that needs some good community assistance; so its always good to explore a little bit on [Github](https://github.com/explore), [SourceForge](https://sourceforge.net/), or simply googling "[open source kart game](https://supertuxkart.net/Main_Page)", before choosing the software you'll be sticking with.

## Second step: Foundations of Building

This is a simple first step that often comes unnoticed when thinking for the first time "How can I contribute for this software?" - and it is how to build said software. Usually it is a small learning slope, but if you are a complete newbie, than this will become a threat to your will of making the FLOSS world a better place. So I can't recommend enough *reading the building tutorials for the platform of choose*; there is usually a page like that on the Software wiki, but if there isn't, <strike>may God help you</strike> than you'll have an early homework on your Google skills. On the most troublesome case scenario, you'll have to *ask the community* how to build the software; usually it comes to a forum, but even an email to someone that monitors the project may suffice. But you'll be building and rebuilding the project a lot, so take your time understanding the tools at your disposal before trying an adventure on the code.

Also, almost every Open-Source software uses some kind of versioning system; be it git, svn, mercurial, etc..; and this is something crucial in how the whole system works, because by the end of the day you want your hard worked fixes to be merged on that single monolithic software you downloaded and built. Being fluent in one of those versioning systems sure is going to help you a lot along the way, but you can handle yourself if you know one of them *just enough* to do the following:

1. Download the most updated version of the software.
2. Modify it at will.
3. Commit those changes.
4. Submit those changes _somewhere_.

Godot Engine is hosted on GitHub, so it was trivial to set up a fork project on my account and start branching for the issues from there; if you are eager to start, I really recommend looking for some GitHub projects, as it has a really quick setup and a user-friendly Pull Request system (PR's are basically you requesting the project owner for him to 'pull' the modifications you did on the project code to the official repository; hence, "Pull Request"). Other projects may have different PR's or merging systems; if they do, look for tips or guides on wikis, and stay close to the project development community so you can solve the project setup issues quickly. Remember, this is not the thing that you want to waste your time with as you are not really delving into software solutions, so just stick in understanding what you have to do to make things work. Setup may be harder than it looks, but after you solve it for the first time, the real fun begins.

## Third step: Tackling the bugs.

Along my Hacktoberfest 2016 journey, I made some annotations on what could be useful information for anyone starting for the first time the same adventure as me; it's mostly simple stuff, but there are some good programming contribution principles I learnt in each PR I made. So I'll be going through each of the PR's, with just a small context explanation before the hopefully helpful advice.

### [First PR](https://github.com/godotengine/godot/pull/6802): Adding a plus sign to parser

The first PR I did was a small usability improvement on Godot script language GDScript: I'm used to insert a '+' sign before positive values as a visual indication that said value is relative-to-something (e.g. "move(+5)"), and I wanted this to be possible on GDScript too. It's absolutely not core for a script language and it is mathematically redundant; but it's not wrong, and it is a visually acceptable way to look quickly at a code and understand faster what it does. So after some deliberation with other developers, I started working on this, even if there wasn't any Issue on GitHub asking specifically for that.

The very first step was discovering in which file the script parser magic happens; a simple search on GitHub showed me the results. From there, I tried understanding the basics of the file, until I found where exactly in the code the script parser was interpreting signed values - certainly there was already the '-' sign parsing, so all that I had to do was following the entire piece of code and copy this behavior to the '+' sign, only changing the part where it actually inverts the value.

**+ Discuss your contributions with the community.** Don't be shy when thinking on cool enchancements you can do on the software - throw the idea to the community, share it so you can have an idea how much effort your idea is worth of.

**+ Don't reinvent the wheel.** This is a programming advice that is true almost 100% of the time, but in Open-Source software contributions, it has an added effect; you want the code you are writing to be similar to the code already written, or else with every contribution you will have a monster file with different coding styles and patterns used. Check for the methods already created, be sure you know if you can reuse something before creating anew.

### [Second PR](https://github.com/godotengine/godot/pull/6847): Correcting an Issue about unintended Input after Crashing

A simple issue regarding how the Text Editor would have the focus just after a game on the Engine crashes, meaning that sometimes you would have unintended input on the script when a game you were testing in Godot stop functioning. It looked simple, so I initially tackled this thinking on how I could take the focus out of the editor; the exact investigation process was the following:

1. Where it sets up the Editor? Editor.cpp
2. Didn't found anything about Focus nor a signal about it. Searched on it about github, to see any mention of the code that might be useful.
3. Learnt that grab_focus is used by viewports and such, but still not what I'm looking for. Start searching for anything related to "play edited scene", to see if there is a way to change focus on the same code that plays the scene.
4. Editor_node.cpp has the code I was looking for; it plays the scene! Start looking at "_run()" to see what I can use there.
5. By the end of "_run()", found out what looked like what defined the moment in which the user pressed the play/test button. By looking at editor_node.h, I tried finding what could be used to change focus when play was pressed; and a couple of tries and rebuils resulted in nothing. Maybe there was something more that I was missing, but just before the code, there was an "emit_signal("play_pressed")" - maybe the class that grabbed this signal was the one using it to actually run the game?
6. By searching "play_pressed" on github, I found out that "play_pressed" was being connected to the method _editor_play! Apparently, it is just a simple method that pops up the debug-menu stuff. Again, by looking at its .h file, I presumed that maybe the debug_menu popup could grab the focus, taking it out from the editor and correcting the problem of unintended input. It worked, and I only needed to add a single line!

**+ Simple is elegant.** Don't overextend a solution for a simple issue if you can resume it in few lines, as long as they make sense.

**+ Sometimes it is an investigation game.** Be sure you know the tools at your disposal so you can jump easily from a file to another (GitHub can search for keywords accross all of the repository files, for example). Sometimes this is the only thing you need to understand a problem.

### [Third PR](https://github.com/godotengine/godot/pull/6883): Changing script extension during its creation

This was a simple usability bug, in which changing the type of the script without changing its extension wouldn't be alerted by the Engine. Like my second PR, I found out what I needed to change by investigating accross the files where the Editor checks when a user is changing a script name during its creation. It was a specific part of the code that could be solved just taking a line out of an if-statement; but after thinking about this problem, I thought it would be much more useful if the Editor automatically changed the extension of the script you were creating for you, since it already does not allow the user to create a script with the wrong extension. I made some mistakes during the solution process, but at the end I could create exactly the usability I was looking for while solving the core issue.

**+ Some solutions can be improvements instead of fixes.** It's always fine to take your time thinking about the problem to be sure whether just fixing it is the best contribution you can make for the issue at hand.

**+ Be sure to mantain contact with the community when PR'ing.** The original solution I thought about had an obvious flaw I didn't actually realised myself it could happen (check the GitHub PR history for more information about that); gladly, I was warned about it, and I could fix it and send again the correct version of my contribution without any more trouble.

### [Fourth PR](https://github.com/godotengine/godot/pull/6962): Changing editor tabs with an External Editor

The last PR I made during Hacktoberfest 2016 was a solution to another usability issue: if you set up an External Editor on Godot Engine, it would pops up after requesting to edit any of the script files - but the Editor itself would also change tabs to its own Script Editor, even if it would be empty in that case.

This last PR was more troublesome to solve not only because it took me a long time figuring where in the code the Edit magic happens, but also because after finding it, I spent quite some time figuring out the 'most elegant' solution. In the end, I found it would be better if I treated the issue as an exception than modifying a lot of a code that was already working pretty well - so I just added an if-statement to check if the user was trying to modify a script AND there was an external editor to use; in that case, just edit the script without changing tabs.

**+ Special case scenarios can happen.** It's always good to generalize programming solutions as elegant as possible, but if the issue simplicity balances out the number of already consolidated lines you would have to change, than it's best to solve the problem by treating it as an exception. 

## Closing remarks

Participating in Hacktoberfest 2016 was one of the best learning experiences I had; not only because of the sheer amount of experiences I accumulated by having contact with high quality open-source code, but because it felt really productive along all the way. The most troublesome obstacle is actually starting the contributions; the whole setup can be burdensome, and spending time learning to build a software is noway as fun as solving the real issues. But as far as newtonian physics analogies goes, after you start moving, it's harder to stop than to accelerate.
