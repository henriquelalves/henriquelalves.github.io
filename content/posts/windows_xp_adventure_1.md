+++
title =  "Windows XP Retro Gaming Part 1: It begins"
date = 2023-08-24
[taxonomies]
tags = ["windows-xp","retro-gaming"]
+++

*I'll be posting my adventure trying to create the perfect Windows XP machine just so I can play The Sims 1 (and some other games, but mostly The Sims 1)*

I recently mentioned on Mastodon how tempted I was to buy an old machine to start retro-gaming with Windows XP. The main reason is this:

![The Sims and Civilization IV](/posts/windows-xp-adventure-1/the-sims.jpg)

I found this refurbished The Sims 1 (and Civilization IV) on a second-hand store, and for me this was quite a find because:

1. The Sims 1 is one of the games I most played in my youth.
2. You can't legally buy The Sims 1 in any digital store anymore. [That's also true for The Sims 2](https://www.reddit.com/r/sims2help/comments/sxqye7/getting_the_sims_2_on_2022/).

The second reason is absolutely insane but [not exactly novel](https://gamehistory.org/87percent/). The product reason behind this is, most possibly, the lack of support for playing old games in new hardware. But this is still a shitty reason, because as a savvy user, I don't even have a choice to buy the game with no support whatsoever.

I'm quite fond of digital conservation, so for a €2.00 coin this was a no-brainer to me, even if my only machine was a Manjaro Linux and I wasn't planning to change it to Windows. But alas, my impulsive tendencies got the best of me:

![A Toshiba Laptop](/posts/windows-xp-adventure-1/toshiba.jpg)

This is a refurbished Toshiba laptop I bought for cheap (at least as cheap as buying second-hand electronic in Helsinki goes). It's a **beast** of a machine featuring a powerful Intel Core i7 (2nd gen), an NVidia GPU (Quadro 1000M), and 4GB of RAM. I had to look for specs almost exactly like this (a 2nd generation CPU, an old Quadro GPU) to have some guarantee that I would be able to find Drivers for them when running Windows XP.

The Laptop came with the digital version of Windows 10, though. Why I'm so fixated on Windows XP?

1. Windows in general have a good reputation on maintaing compatibility with old software, but Windows 10 is a bit too much new for the software I want to play. I tried installing a Diablo 2 CD (also bought for 2 euros) but I wasn't able to run the game.
2. Even so, there would probably be fixes for most games I'd like to play (Tower of the Sorcerer, another game I really wanted to play, worked flawlessly on Windows 10 - and this is a game I couldn't run on Wine because of font issues) - but that is not a guarantee. I want a guarantee.
3. Windows XP was one of the most popular OS of all-time (source: my intuition) - and is also a great OS to run the games of the era I want (plus it has great compatibility for even older games).
4. And honestly, playing stuff on Windows XP is part of the charm of the whole process. I want to feel this nostalgia.

The Laptop itself came with Windows 10, so I downloaded a legitimate copy of Windows XP (which [apparently Microsoft itself uploaded to Internet Archive](https://archive.org/details/WinXPProSP3x86)), used Rufus (on the Windows 10) to create a bootable USB, booted from it, and voilá!

![Installation screen of windows XP](windows-xp.jpg)

It didn't work!

![BSOD during windows xp installation](oops.jpg)

So, there is ONE part of the laptop spec that is actually quite troublesome for installing older OS's: the fact that it uses an SSD.

On my quite short research, it seems that SSD drives don't have some of the commands HDD have (in particular, TRIM), so Windows XP doesn't exactly know what to do once it tries to install on it.

I still have quite a bit of investigation to do, but it seems like I can install 3rd party drivers before installing the Windows XP, and one of the drivers might be the SSD driver so I can actually continue with the installation. So my options right now are:

1. Make due with the SSD. It may possibly deteriorate the SSD faster though, because older OS's weren't made to use SSD intelligently (and old SSD drives weren't as robust as the ones we have today).
2. Replace the SSD with an HDD. I'll have to check the Laptop adapters though (I know it's possible, but I have no idea if the laptop age is going to get in my way).

There you have it. Hopefully the following post will have some answers on how the hell I'll be installing Windows XP on my Toshiba.
