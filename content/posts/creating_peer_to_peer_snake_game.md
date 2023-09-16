+++
title = "creating a peer-to-peer snake game with godot webRTC"
date = 2023-09-07
[taxonomies]
tags = ["making-of", "webrtc", "multiplayer"]
+++

*I posted this on my "write.as" profile (which I don't use anymore) on 2021-01-06. I still plan to update the project (the server I made is down, and I have a couple of ideas to improve it), but I'm re-posting this guide as-is*

# Creating a Peer-to-Peer Snake Game with Godot WebRTC

This is an introductory article on creating a simple P2P web browser game using the Godot Game Engine. It assumes the reader is familiar with the Engine and has at least intermediate programming knowledge.

The project is a very simple Multiplayer Snake game played in versus; each player controls a Snake, trying to outgrow the enemy player and force them into a corner. The last player standing is the winner.

![Game Screenshot](https://i.imgur.com/N6kT1hS.png)

The project source-code can be found [on GitHub](https://github.com/henriquelalves/SnakeVersusWebRTC); the result is playable on [Itch.io](https://perons.itch.io/snake-versus). This article only provides context for parts of the source code.

The project was inspired by the WebRTC example available on Godot Demos repository.

## Why Peer-to-peer?

There are a lot of strategies to create multiplayer games, and most of them fit into two online architecture categories:

- *Authoritative Server*. Players connect to a Game Room server, which becomes the single source-of-truth for the game simulation.
- *Peer-to-Peer*. Players communicate with each other relying on every player having the same game state and every action being performed deterministically.

Detailing both architectures and other implementation strategies (e.g. Rollback, Client-side prediction) is beyond the scope of this article; suffice to say the *Authoritative Server* model is the most common in production games for reasons like being harder for a player to cheat the game (it's easier to tamper with internet packets in P2P games), and internet connection for all connected players being more reliable (unlike P2P games, in which players depend on each other's connections).

But an *Authoritative Server* imposes some implementation challenges:

- *Scaling Game Rooms*. In match-based games, the number of concurrent matches is not constant, so the number of Game Rooms should be scaled accordingly.
- *Client and Game Simulation difference*. In the *Authoritative Server* model, the game simulation only runs in the server, and the Client is usually a "dumb-client" (only renders the game state and sends the player's input to the Game Room). This difference makes it harder to implement a common code-base for both Server and Client.
- *Server Latency*. Server location can affect the player's online experience differently.  Games that rely on real-time communication with low latency and smaller number of players (e.g. Fighting Games) usually use a P2P architecture for this reason.

For a small project, an Authoritative Server can quickly become a hassle to implement, as it requires a more robust codebase and more expensive backend to allocate Game Room resources. For this reason, a Peer-to-Peer architecture was chosen for this project, as it would be easier to implement the game on a single repository with fewer server resources.

## WebRTC

WebRTC is a new web project that allows efficient peer-to-peer data communication using a common protocol between peers.

From the official [WebRTC website](https://webrtc.org/):

> With WebRTC, you can add real-time communication capabilities to your application that works on top of an open standard. It supports video, voice, and generic data to be sent between peers, allowing developers to build powerful voice- and video-communication solutions.

![WebRTC Data traversal](https://www.html5rocks.com/en/tutorials/webrtc/basics/dataPathways.png)
*(image from https://www.html5rocks.com)*

One of the challenges of Peer-to-Peer communication is handling NAT traversal: most peers are behind NAT walls (e.g. an internet router in their homes), which protects the user from remote internet packets of unknown origins (that's the reason some games that allows multiplayer via a host (e.g. Minecraft) requires the hosting player to "Port-Forward" their router, so other players can connect to the host without the host's router blocking the packets). There are strategies to work around this challenge (e.g. [UDP Hole Punching](https://en.wikipedia.org/wiki/UDP_hole_punching)), but they require a non-trivial setup between clients, and it may depends on the peers NAT configurations. WebRTC is implemented such as the protocol itself will handle the peers NAT traversal using a special "handshake" exchange between peers before allowing them to exchange data.

## Project Architecture

The game is a single Godot Project that contains both Client and Server scenes (the project runs the correct starting scene thanks to a different Export configuration). The project can be divided in *Matchmaker Server code* (a standalone server that will connect players and act as a WebRTC setup intermediary), the *Multiplayer Framework* (client classes that operate on how messages are relayed) and the *Gameplay Code*.

![Example of Application flow](https://i.imgur.com/We4iVUN.png)

The matchmaking flow is:

1. *Player 1 Searches for a match*. The player's client connects to the Matchmaking Server using a WebSocket, and waits for a starting-match message so it can initiate the WebRTC handshake with other players.

2. *Player 2 Searches for a match*.

3. *The Matchmaker pair the players*. The Matchmaker removes both players from the matchmaking queue, and sends to both players a "start match" message with each players information.

4. *Player 1 sends an Offer (first part of the WebRTC handshake) to Player 2 using the Matchmaker server as relay*.

5. *Player 2 sends an Answer (second part of the WebRTC handshake) to Player 1 using the Matchmaker server as relay*.

6. *Both players wait until a connection is stabilished thanks to the WebRTC client polling*.

7. *Players disconnect from the Matchmaking Server and start a match using the WebRTC Peer-to-Peer connection*.

### Matchmaking Server

The Matchmaker is the only standalone server the project needs, so it can pair players looking for a match and relay their WebRTC handshake. It's game agnostic (it has no knowledge of the game data exchange) and works as a relay server as long as players are connected to it. This makes the Matchmaker very lightweight, thus needing very few resources to work.

The Matchmaker is a simple scene that runs the `MatchmakerServer.gd` script.

![Matchmaker Ready Function](https://i.imgur.com/LpnljtX.png)
*MatchmakerServer.gd*

The players connect to the Matchmaker using a WebSocket port (the reason being that WebSocket's are easier to setup and exchange messages than a TCP/UDP connection). The Matchmaker stores the players information in a queue, so it can poll from it later to pair the players. When the number of players in the match queue is bigger than the match size (a configurable editor variable), the Matchmaker creates a match by sending to each player the necessary information to start a match (each other' players connection ID).

![Message on Server](https://i.imgur.com/OQENLJ4.png)
*MatchmakerServer.gd*

The `Message` class (`Message.gd`) is the only common class between the Client and the Matchmaker server; it's used as a wrapper for the packets the players send to the network by converting a variant to an array of bytes using Godot's `var2bytes` standard method, and wraps it to send as a packet along with useful flags (such as a "start match", or "echo" messages that should be sent back to the sender client). When the Matchmaker server receives a message from a player ID, it relays the message to the other players connected in the same match.

### Multiplayer Framework

The Multiplayer Framework consists of:

- `ClientManager`. Responsible in setting up the relay messages with the Matchmaker Server using WebSockets and setting up the WebRTCClient class signals when a match starts.
- `WebRTCClient`. The proper WebRTC client class that sets up the Peer-to-Peer connection after a match starts.

![Connect to Server](https://i.imgur.com/dIXUS9l.png)
*ClientManager.gd*

When connecting to the Matchmaker websocket, the ClientManager sets all match variables (where players information that comes from the Matchmaker are stored), and which number the player is (used to set which player is the "host" in the Gameplay code). The ClientManager also has both the WebSocket and WebRTC clients and sets their callbacks accordingly.

![WebSocket Client](https://i.imgur.com/JUEGPKz.png)
*ClientManager.gd*

The business logic for the Websocket client is hardcoded in the ClientManager class, and it deals with initializing the RTC client when receiving a message from the server with the `match_start` flag set. This creates an RTC peer, and set it up to receive the RTC Handshake messages.

![Create Peer](https://i.imgur.com/xj3FC9S.png)
*WebRTCClient.gd*

When creating a WebRTC peer, we need to provide the "ICE" server we're going to use (a third-party server responsible in setting up the NAT traversal protocol); there a freely available ICE servers such as the one used in the project. The WebRTC clients are divided in two groups: the *Offer* group (clients that will create an offer and send it in the network), and the *Answer* group (clients that will wait for an offer message, and then answer with their network information). On this stage of the process and until the WebRTC handshake finishes between all clients, all messages are exchanged via the Matchmaker Server.

When all peers are connected, ClientManager will emit a signal so that the match can start, and the gameplay code disconnects from the Matchmaker Server (as it will send messages directly between players using the stabilished WebRTC Peer-to-Peer connection).

### Gameplay Code

![Gameplay Code](https://i.imgur.com/FWN3pb6.png)
*Game.gd*

The Gameplay code itself is very simple and naive Snake game; it actually uses one player as a match host and the source-of-truth for the simulation (a client working both as a dumb-client and as an authoritative server). When the match starts, the host will send the seed for the simulation (so all players get the same output for the Food positions). During the course of the match, other players will just send to the host their inputs, and then the host "ticks" the simulation, sending back the "final" input of each player for that particular tick, effectively moving every simulation to the same next turn (as all food tiles will have the same position in all clients, so the simulation is deterministic). As soon as there is only one player standing, the match ends (since it's a two player game for now, there is no problem with the host leaving the match, as it would end either way).

## Thoughts about the project

I started this project so I could create a simple framework for browser multiplayer games with the Godot engine; originally, the "Matchmaker Server" was a Relay server that players used throughout the match to communicate with each other (a "fake" peer-to-peer, as the clients were using the Relay server to communicate with other peers, but the server itself was still agnostic to the game being played). The main problem was latency; there was no cheap server to rent in South America (where I live), and communication was really slow between peers with a Relay Server in North America.

I learned about WebRTC looking into Peer-to-Peer solutions for the Web, and was amazed by the fact that Godot already had it implemented in the Engine. It did took me quite a bit of time to figure out how the `webrtc_signaling` demo worked, as the documentation was somewhat lacking, but the payoff was a fraction of the latency I had with the same Snake game with a Relay Server.

This project is by no means feature complete (it has obvious design flaws, like the fact that a snake can run in circles indefinitely, effectively deadlocking the match), but hopefully it can help anyone else also interested in creating Peer-to-peer games. It's amazingly easy once you understand and implement the framework that'll support the game itself.

Cheers!
