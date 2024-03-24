+++
title =  "windows XP retro gaming part 3: finally, success"
date = 2024-03-24
[taxonomies]
tags = ["windows-xp","retro-gaming"]
+++


After [last post](/posts/windows-xp-adventure-2/), I decided on replacing the SSD with a Sata driver of 1 TB. Long-story short, this wasn't enough because I was still trying to install Windows XP via USB - so I spent 20 bucks on a DVD Writer that connected to my actual-modern computer via USB, burned a Very Legitemate copy of Windows XP from [archive.org](https://archive.org), and finally used it to install Windows XP.

![Fujitsu installing Windows XP](installing-windows.png)

After burning another CD with Fujitsu Windows XP drivers (that fortunately are all available on their website), my Windows XP gaming machine is finally ready! And it can connect to the internet via wi-fi! But, my only browser is Internet Explorer 6.0, and it can't connect to most websites with the exception of google homepage.

![Google homepage on Internet Explorer 6.0](google-on-ie.png)

Having a better browser would be really handy to solve some problems installing retro games on it, so using Google (the only website available), I searched for a Firefox binary on archive.org, and after a little bit of trial and error, I managed to open the archive.org page for an older Firefox binary (basically, I had to remove the "Secure" from the HTTPS to make the webpage visible enough on Internet Explorer 6.0).

![Downloading Firefox](downloading-firefox.png)

And now I finally had a machine that could browse internet (a bit) and install retro games! Some lessons learned:

- Installing Windows XP on SSD is hell. Replace it with a <2TB Sata HDD.
- Installing Windows XP via USB is hell. Burn a (Very Legitemate) copy of Windows XP installer on a CD. You'll also get to hear the CD burning grinding song of ages.
- First Windows XP installation didn't work because I had the wrong Boot mode on my Bios. Changing it to "Legacy" and it worked flawlessly.
- If you want to retro-game, install Windows XP 32-bit! I made the mistake of installing 64-bit first, and some abandonware games I wanted to test didn't run on it because they had 16-bit installers (and 64-bit Windows wasn't compatible). 64-bit Windows also made some games run slower (e.g. The Sims 1), and I don't think there was a game from the 2000 -2010 era that didn't work on 32-bit Windows XP.
- I didn't check before but I got really lucky that Fujitsu website made it super easy to find the exact drivers I needed to run Windows XP on my machine. It's a very obvious "oopsie daisy!", but before commiting on buying a machine, check how easy you can find its drivers for Windows XP.
- Before having internet connection on the notebook, I had to install the drivers (that I burned on another CD). Interesting enough, a lot of the drivers I tried installing didn't work - the reason was that I extracted the drivers `.zip` on my archlinux and burned the Data CD there, but in the process, the files were "flattened" on the disc (files in subfolders moved to the parent folder, and the subfolder disappeared). So I burned another Drivers disc with the files still in their `.zip` folders and opened them on Windows, installing directly from the `.zip`.

Now, finally, I could play a bit with my Windows XP. Since the laptop's webcam was working, the first thing I did was to check The Sims 1! Why, you ask?

![The high quality photo of myself from the Webcam](me-webcam.png)

This is me!

![A screenshot of my character in The Sims creator](thesimscreator.png)

This is me in The Sims Creator!

![A screenshot of my character in The Sims](thesims.png)

This is me in the game!

It's the ugliest Sim I've made but it did made me laugh quite a lot (the left Sim is my wife that was obviously Simified from a terrible Webcam photo too).

The other game I installed was Diablo 2, that runned flawlessly out-of-the-box, no patches needed.

![Diablo screenshot](diablo2.png)

I never finished Diablo 2 before, so this is also a game I'm expecting a lot.

I already burned  some `.iso`s of CD Games magazines me and my wife had in Brazil when we were kids - lot's of those CD's are from a now-dead publisher called Digerati, and had a lot of game demos and Flash game nonsense from that era. I also want to try my hands at making a game in Game Maker 5.0, the first Game Maker version I've used and the software that made me realize that making games was a lot of fun. Maybe this is worthy of another post, something like "making games like it's the 2000's", but I think I'll try finishing Diablo 2 first with my vanilla "lots of skeletons" necro build.
