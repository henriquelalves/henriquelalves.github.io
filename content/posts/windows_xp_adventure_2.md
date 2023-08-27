+++
title =  "Windows XP Retro Gaming Part 2: Trying to create an ISO"
date = 2023-08-27
[taxonomies]
tags = ["windows-xp","retro-gaming"]
+++


Before trying my hands again at solving the SSD error, [@adamscott suggested in my Mastodon post](https://mastodon.gamedev.place/@adamscott/110952935538175974) to use [86Box](https://86box.net/) to emulate the Windows XP OS in my hardware, instead of installing Windows XP. It's a software I never heard before, so I investigated it a bit. It was a good suggestion, but I could see those problems:

- It seems like Windows XP emulation [had some problems in 86Box](https://github.com/86Box/86Box/issues?q=is%3Aissue+windows+xp+is%3Aclosed). This made sense to me - 86Box seems like it was made to emulate older OS's, and Windows XP is kind of between the newer OS's that should run 86Box and the old OS's it was made to emulate.
- It still is an emulation layer between running Windows XP in my hardware and the software I want to run. I couldn't have any guarantee that it would run all software.
- I want to understand how difficult it can be to install Windows XP on the hardware I have.

So I dropped the option of using 86Box and went back to look on how to create a bootable Windows XP USB with the drivers I needed.

I started looking into lots of forums and tutorials, and the best one I found was this one: [https://windowspro.eu/installing-windows-xp-on-ssd-disk/](https://windowspro.eu/installing-windows-xp-on-ssd-disk/).

### First Round: Enabling BIOS Hard Disk compatibility mode

The tutorial mentions that I could enable BIOS Hard Disk compatibility mode so I could install Windows XP anyway, and later change the HDD operating mode with registry tweaks. Seemed like a no-brainer to me, but it didn't work. I had the same `7b` BSOD problem as before.

### Second Round: Creating a Bootable CD with the drivers I needed

So, I tried the next tutorial option: to create a Bootable CD with the drivers I needed.

BUT, **fun** stuff: I can't create a Bootable CD, because the DVD reader from my laptop was just that: A DVD reader. Not writer.

So, I had to move forward with the assumption that a Bootable USB would work the same as a Bootable CD, which may as well be a completely wrong assumption (during Driver installation, the setup is specific in saying it's looking for extra drivers in the CD or floppy disk (lmao)).

### Second (and a half) Round: Creating a Bootable USB with the drivers I needed

More **fun** stuff happened now:

- I tried looking into the drivers for my SSD (a Toshiba THNS256GG8BBAA) in a bunch of shady sites, but I couldn't find a download that gave me the actual driver (I may have download tons of viruses though, maybe, possibly). But even the shady sites only showed drivers for Windows 7+.
- nLite, the software the tutorial mentioned is used to create the "updated" bootable Windows XP setup WITH the drivers, didn't work with any Windows XP ISO I downloaded. I'd find later an anti-feature list that pointed out that nLite wouldn't create Windows XP ISOs, so I have no idea the tutorial was just wrong or if it was assuming a different (older) version of nLite.
- the OTHER software this tutorial mentioned could also be used to create an updated Windows XP ISO with extra drivers, as mentioned in this second tutorial (that the first tutorial even mentions, making the whole process a bit confusing to me. Why the hell I needed nLite for, then?): [http://forum.driverpacks.net/viewtopic.php?id=1449](http://forum.driverpacks.net/viewtopic.php?id=1449). BUT:
  - This second tutorial has broken image links, so I couldn't check if the process was going the way it was supposed to or not.
  - The software in question, DP_Base, downloaded updated drivers from its Database. The Database is offline though. So no extra drivers (I downloaded a "Drivers pack" from the first tutorial though, with supposedly all the drivers I needed).

Using DP_Base, I finally created a new version of the Bootable Windows XP USB I was using, set the installation to install drivers before installing Windows XP.... and it didn't work, because it couldn't find any drivers to install. I tried using another Windows XP base ISO just to be sure, but to no avail.

This was no surprising to me at all, since the whole process was convoluted as hell, I couldn't find good sources of information, and I was relying on software that is not even supported anymore to create a Bootable USB of an OS that is even less supported. It's part of the course, but it reminded me that most information on the internet is not made to be preserved. Maybe if I bought the Fujitsu notebook and tried to install Windows XP on day 0 back in the day, I'd be able to find the drivers, software and information I needed. In 2023, that's definetly not as easy, and maybe just not true at all.

I'm not so much of a masochist as to keep fighting my own SSD just so I can install Windows XP, so onwards to plan B:

![Fujitsu without its bottom part](fujitsu-naked.jpg)

![Fujitsu without its bottom part](fujitsu-ssd.jpg)

I checked how to remove the SSD from it, and I'm planning to replace it for an HDD. I double checked and it seems like Windows XP (with SP1) is compatible with SATA III under 2TB, so I'll buy something on the range of 1TB (probably? It's the least and cheapest SATA I could find) and replace the SSD.

Let's hope for the best.
