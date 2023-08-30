+++
title =  "Mastonews Devlog 1"
date = 2023-08-30
[taxonomies]
tags = ["mastonews","devlog"]
+++

I published [mastonews.com](https://mastonews.com) a couple of weeks ago but I don't have yet written anything about it's creation process. Mastonews is a personal project that I ended up learning a lot and made me lose a bunch of fears of publishing a web project that is not game related, so I think it may be benefitial to write a bit about it. It's also going to be interesting to check in the future how the project unfolds.

I'll divide this post in my initial objectives, what I tried, what I ended up with, and the current project status.

### Objective

The objective was to create a website that reads your mastodon profile and creates an RSS feed from the personal blogs and websites of the accounts you follow. I've talked a bit about where this desire comes from on a [previous post](/posts/notes_about_making_the_web_weirded), so the brief explanation is that the final idea of Mastonews is to help the discoverability of new and interesting websites by leveraging users you already trust.

### The first steps

The idea of Mastonews is actually quite simple and could as well be just a python script:

1. Use Mastodon public API to get all the accounts that a public account follows.
2. List all the verified links of each account from the last step.
3. List all RSS feeds of each verified link listed from the last step.
4. Display the content from all the RSS feeds in chronological order

Step 4 is pretty much optional as a python script, I could just write all RSS feeds into a file and use this file in any other RSS aggregator. But although a python script would be really simple to make, it would never become something that non-tech people could use openly, so I never went forward with just making it a script (it would also become a problem the fact that Step 1 only works for public accounts; private accounts would need OAuth, and thus a domain to be redirected to). Instead, I jumped right into making a simple FastAPI server to test how I could talk with the Mastodon API from my backend, and get ready to display it in a web page frontend.

### Trying FastAPI

[FastAPI](https://fastapi.tiangolo.com/) is a nifty Python package to create efficient API servers. Although I'm more or less used to Django, I really wanted to learn FastAPI because it seemed like a shiny new Python toy, so I gave it a spin. It helped that this project looked simple enough to test how a production FastAPI environment would be.

Recreating the behavior from the last step was really simple, but I notice a problem in how the Mastodon API would work really early in the project: it (understandably) have really low API call limits for single IP addresses (maximum of 300 requests in 5 minutes). And to make matters worse, the only way I knew how to get all the account follows from a single account (and to this day) was to get paginated results with a maximum of 80 accounts - so I'd have to request multiple times for each user.

This was a blocking scalability problem to me, since I wanted my app to become popular (not today, but surely tomorrow). So I reviewed my strategy, and decided that if the frontend made the API calls to get the follows, I wouldn't have this problem anymore (since it's the single user IP that is making the call, so hitting the request limit is much less probable). I'm not sure if this is an obvious web application setup I was just unaware of, but to the web clueless me, this was an insight I had to have.

So now I'd have to choose a frontend library.

### Trying SvelteKit

I knew [SvelteKit](https://kit.svelte.dev/) from before, but I never created any project with it. I knew it from React alternatives I was looking into, because for some reason I just can't create a mental model of how the hell React works, so every time that I tried making a "fun little project" in React, I just got intellectually sad and demotivated.

I really don't know why my brain hates React so much, but I accepted that I'm not a React guy and moved forward with my life.

SvelteKit seemed like a really good alternative. Does it have a mistery black-box (the compilation process) that transform all of your stuff into other stuff? Yes. But from what I looked into and tested, I could pretty much trust the black-box and just worry about writing some HTML and JS and CSS. The responsiveness would work out of the box and much more intuitively to me than with React.

The other thing that make SvelteKit attractive was the fact that the backend could be thrown in together on the same development process, making the hobbyst life much easier, AND the backend would be written in JS too. I have nothing against JS in particular (I even have friends that like JS), but what gravitated me towards a JS backend was the fact that there were more libraries to look for RSS feeds in arbitrary URIs in JS than in Python, and this was a problem I didn't want to solve myself.

So SvelteKit was chosen, along with [skeleton.dev](https://skeleton.dev) UI. I ended up with this architecture:

- Frontend: Makes the API calls to list all accounts, list all verified links, send the list of verified links to the backend, and waits for the response to list all content to the user.
- Backend: Receives a list of links, search for the RSS feed in each link and list its content (the posts), and returns to the frontend a list with all posts made in the last 24h.

Notice I wouldn't be able to make everything happen in the frontend because of the RSS-feed-search step. CORS protection won't allow a browser to make arbitrary requests to websites (DISCLAIMER: I'm web clueless and I have no idea how CORS works, but I did tested this and CORS didn't like what I was doing).

### Current Status

The project is still under development. The last bit I made was to allow private accounts to use Mastonews by way of OAuth, but for the short term I still have this in mind:

- Add some structure to the website (About page, Donations page, etc).
- Filter buttons for each account that is shown in your feed (so you know how many posts each account is showing and can filter them).
- A "popular" / "trending" list of accounts and their websites, to make discoverability more interesting.
- A small (opt in) donation incentive of showing users that donate to the website with a small flair around them.

In the long term I have bigger plans for the project, but I want to make sure the project is self-sustainable via donations before anything bigger, so I don't need to worry to monetize the website in any other way.

That's it for now. There still some things to write about (hosting the website, website logging and stats), but I'll leave that to a second post.

Cheers!
