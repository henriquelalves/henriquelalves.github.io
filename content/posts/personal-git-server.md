+++
title = "a lightweight personal self-hosted git server"
date = 2024-11-30
[taxonomies]
tags = ["self-hosting", "git"]
+++

For some time I've been looking for a good solution to self-host my own git private server, and start migrating from GitHub. My requisites were:

- Lightweight.
- A frontend for public repositories.
- An easy API to manage my repositories.

Which, to be honest, are not many requites, but even then it did made me scratch my head a lot on the best approach. After playing with some solutions (from vanilla git running on a remote machine and managing it with SSH, to different flavours of Gitea and GitLab), the one I feel most confortable is using just two very simple tools: **cgit** and **Soft Serve**. You can check the (public repositories) end result here: [https://git.henriquelalves.com/](https://git.henriquelalves.com/). I'm still in the process of migrating some of my repositories from GitHub, but I'm confident I'll be using this solution for years to come.

### What cgit does?

[cgit](https://git.zx2c4.com/cgit/about/) is a web frontend for a system folder that contains git repositories. That's it.

It has **no git functionality**, it just controls web requests and spit out a tidy web page with your repository descriptions, and may also be used for HTTP git cloning. Super lightweight, easy to setup and configure, and the defaults are enough for 99% of cases.

It also has NO functionality for Issues/Pull Requests, which for me is a plus. I don't want to receive issues or pull requests, I'm just providing the source code of my software AS IS. If I ever consider receiving Issues or Pull Requests, I'll setup an email account to receive patches.

### What Soft Serve does?

[Soft Serve](https://github.com/charmbracelet/soft-serve) is a `self-hostable Git server for the command line`, per their GitHub page.

What it really does is running a lightweight server on your machine that you can login via SSH, and compared to using vanilla git on a remote machine, it's basically simplifying the process from "Login via SSH to my remote machine and manually setup and configure git bare-repositories" to "call `ssh soft` on my local machine and manage everything on a pretty TUI".

It also provides some cool tools to manage private repository access for different users (different SSH keys). I never used this feature, but it's a nice-to-have that gives me some peace of mind.

### Why both tools?

**cgit** has no git functionality, but it's a good lightweight git repositories frontend.

**Soft Serve** is a great (and also lightweight) git server, but it's a tool for the **developer**, and provides no public interface for the repositories.

### How to manage both tools?

The way I use the two tools, I have:

- A root repositories folder on my system (e.g. `home/git/repos/`)
- Every repository is managed by **Soft Serve**. Repositories I want to be public are named `public/[...].git`, and private repositories are `private/[...].git`. That makes all repositories fall into either `home/git/repos/public/` or `home/git/repos/private/` subfolders.
- **cgit** scan path is `home/git/repos/public/`. It has no access to the private repos, and nor does it care.

Done and done.

**Soft Serve** has it's own server process that you run with a `soft serve` command, while **cgit** runs as a [CGI](https://en.wikipedia.org/wiki/Common_Gateway_Interface) program. Since Soft Serve uses exclusively SSH (and you'd be using the terminal to do so), you don't need to worry about reverse-proxy and whatnot - you can just run Soft Serve on a VPS with the ports it needs open, and configure your client `.ssh/known_hosts` (so you don't need to remember the IP address all the time). Soft Serve README does a really good job explaining the whole process and you shouldn't have many difficulties setting it up on a VPS. The one thing I did extra was to create a `systemd` service for it (so if its process ever crashes, it will restart).

**cgit**, on the other hand, needs a reverse-proxy. I used two tools:
- [Lighttpd](https://www.lighttpd.net/) running on a local (not public) port, redirecting to the cgit cgi.
- [Caddy](https://caddyserver.com/) reverse-proxying my domain to the local Lighttpd port.

There are certainly simpler solutions, but Caddy is in my comfort-zone when I don't want to think about SSL certificates. Caddy itself has no CGI support though, hence the "Caddy -> Lighttpd" flow.

Last but not least, it still is a public server, so I recommend installing and configuring `ufw` to block all ports (but the ones you are actually using for **Soft Serve** and serving HTTPS requests via **Caddy**) and installing `fail2ban` on your machine.
