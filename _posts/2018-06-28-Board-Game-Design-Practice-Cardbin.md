---
title: Board-Game Design Practice - Cardbin
---

**TAGS**: Cards, Luck, Turn-Based
**PLAYERS**: 2-4
**DURATION**: 5min


### Introduction
Cardbin is a luck-based card drawing game, in which players compete on making the Bin pile of cards reach the target size shown on the Target card (the top-most card of the Target pile).

### Setup

Cardbin uses only the Spades suit, cards from Ace to 9 (10, J, Q and K should be ignored).

The game begins with the Three of Spades face up (target pile), and the rest of the cards shuffled in a pile besides it face down (draw pile).

![Game Setup](/assets/cardbin/game_setup.png)

### How does it play

After a turn order is set between the players, on each turn, the player of the turn must choose wheter he will put the first card of the draw pile face-up on the target pile, or face-down on the bin pile.

Then, the following will happen accordingly:

<ul>
<li>
If the total number of cards in the bin pile is THE SAME as the number shown in the top-most card of the target pile, the player that played this turn wins.
</li>
<img src="/assets/cardbin/win_cond_example.png" alt="Win Condition Example">
<p></p>
<li>
If the total number of cards in the bin pile is LESS THAN the number shown in the top-most card of the target pile, the player ends his turn and the next player turn begins.
</li>
<p></p>
<li>
If the total number of cards in the bin pile is BIGGER THAN the number shown in the top-most card of the target pile, the player that played this turn loses and quit the game.
</li>
<img src="/assets/cardbin/lose_cond_example.png" alt="Win Condition Example">
<p></p>
</ul>

If on a players turn the Drawing pile is empty, the player must shuffle the Target pile, put the cards face down on the Draw pile, and use his/her turn to put a card from the Draw pile to the Target pile. This counts as an action and the win-condition should be tested accordingly on the end of the turn. The player can't do anything else on the turn.

In the case of a Nine of Spades face-up on the Target pile and no more cards left to draw, the game is considered to be a Draw.

### Tips
<ul>
<li>
Even if Cardbin is a luck-based game, you can improve your chances by knowing which cards are left in the draw pile, and choosing when to test your luck with the target pile.
</li>
</ul>


### Credits

Game designed by Henrique Alves. Rules are CC-BY.

Card art from andrewtidey (CC0): https://opengameart.org/content/cards-set
