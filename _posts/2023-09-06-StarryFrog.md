---
layout: post
title: "StarryFrog"
---

<div style="text-align: center;">
  <img src="{{ site.url }}/images/StarryFrog/StarryFrog.gif" width="50%"/>
</div>
<br>

Try the game live at: [https://daneelsan.github.io/StarryFrog/](https://daneelsan.github.io/StarryFrog/)

At the end of last year, I found out [raylib](https://www.raylib.com) has having a gamejam to celebrate reaching 9 years [https://itch.io/jam/raylib-9-years-gamejam](https://itch.io/jam/raylib-9-years-gamejam).

Until that point, I had never participated in a gamejam (or any other kind of code-related jam), so I thought it was a great opportunity.
`raylib` is (according to its wiki) "(...) a simple and easy-to-use library to enjoy videogames programming.".

In `raylib` the language of choice is C, which is why I discovered it in the first place (at the time I was looking to write a game using C).
Besides having a simple and elegant design, the documentation is really someting.
In particular, I found the [examples](https://www.raylib.com/examples.html) provided to be of great quality.

However, at that point in time I hadn't used `raylib` in any project, so, if not during the gamejam, then when would I?

The theme of the gamejam was: [UNEXPECTED](https://twitter.com/raysan5/status/1598029088299839488).
To me, this is another way to say that there is no theme (although I guess a better interpretion would be to code a game that subverts expectations).
To be honest, at the time I thought this was a bit boring as I find fun having imposed restrictions on projects.
But in retrospective I think it was the best theme I could hope for for my first gamejam.
Besides, I gave my game some self-imposed constraints, like having the screen size be 256x256 pixels, or only using a six-color palette.

At the time, I was playing a lot of [MapleStory](https://maplestory.nexon.net/), where an event was ongoing that had a minigame called [Star Bridge](https://maplestory.nexon.net/news/77917/v-237-ignition-cygnus-knights-redux-patch-notes/#Star).
This minigame was really fun so I thought, why not replicate it for the gamejam?
Not the most original idea I admit, but I thought it would be a great place to start.
Besides, I didn't have the source code of the minigame at all, just the game design so everything still had to come from my head.

I decided to call the game "StarryFrog". The description reads as follows:

	Help a frog connect the stars to create marvelous constellations!

	As the starry frog, your mission is to grab, move and connect stars to form constellations.
	However, not all stars will be able to form a bridge.
	Have a look at the minimap to pinpoint the locatation of valid star bridges in the constellation.

	Finish connecting constellations 3 times to see your score!

Why a frog? At the time I was using a Yu-Gi-Oh! card as a profile picture in Discord: [Treeborn Frog](https://yugioh.fandom.com/wiki/Treeborn_Frog).
Besides being the core card of a deck I used to play, I loved the artwork.
So I decided to use a frog to be the carrier of the stars in this game.
To create the sprites I used [Piskel](https://www.piskelapp.com/), which got the job done.
Here is a the spritesheet I made (after several iterations):

<div style="text-align: center;">
  <img src="{{ site.url }}/images/StarryFrog/StarryFrogSpritesheet.png" width="50%"/>
</div>
<br>

Another topic I should mention is the game constellation maps.
These are the maps that define how stars are layed out in the sky and what stars should be connected to form constellations.
I struggled to write the data of these maps manually, so I used [Wolfram Language](https://www.wolfram.com/language/) to create tools to assist me.
The main function I wrote was `StarryFrogConstellationDraw[]`, which basically opens up a window in which one can draw the maps (and a button that prints out the C-struct data meant to be copied into the game code).
Here is a picture:

<div style="text-align: center;">
  <img src="{{ site.url }}/images/StarryFrog/StarryFrogConstellationDraw.png" width="50%"/>
</div>
<br>

At the end, I didn't win the competition.
Of course, I didn't expect that to be the case, although I thought (and still think) the game turned out to be pretty good.
There are still some TODOs, like adding sound effects or add more maps. To be honest, these probably won't get done ever.
But maybe someone out there wants improve this game, so I leave it to them.
10/10 would try a gamejam in the future again.

Link you might be interested in:

* ["raylib 9 YEARS gamejam" submission](https://itch.io/jam/raylib-9-years-gamejam/rate/1830034)
* [itch.io Release](https://daneelsan.itch.io/starryfrog)
* [Source Code](https://github.com/daneelsan/StarryFrog)
