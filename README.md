# The Fantastic Adventures of Dizzy: Fair Edition
<p align="center">
  <img src="images/FAoDizzy-title.png" alt="Title"/>
</p>

The Fantastic Adventures of Dizzy: Fair Edition is a patch which I created for the NES game The Fantastic Adventures of Dizzy (FAoD). The patch includes several (relatively) simple changes to make the game significantly less tedious. It also fixes a few bug (one of which is especially heinous). With this patch, what was once a nearly impossible game becomes a fun, novel experience which ranks among other classic NES titles.

Change Summary:
 * Dizzy has better temporary invincibility after taking damage (i-frames).
 * Dizzy starts the game with one extra life
 * Spiders do less damage and move in predictable patterns
 * Birds/Bats do less damage and move in (more) predictable patterns
 * Ants/mice do less damage, and mice in the sewers move in (more) predictable patterns
 * Guillotines in the town move in predictable patterns
 * Damage from torches, lava bombs, Triceratops, and Leprechaun are all reduced
 * Added more frames to jump on the log that comes down the waterfall between the town and Yolkfolk tree-village. 
 * Reduced how quickly the Oxygen meter goes down during the bubble mini-game
 * Removed an especially tedious raindrop in the graveyard near the end of the game
 * Fixed a bug where the last star can be collected twice, which soft-locks the player at the very end of the game
 * Removed the (partial) invulnerability glitch
 * Fixed a glitch were lower-priority items are displayed instead of high-priority items (e.g. the star-plant object had priority over the plank object)

 (These changes will be addressed in further detail)

This README documents how to install the patch to your copy of FAoD, as well as what changed with examples. It also includes a lot of technical details of what was changed and how, featuring plenty of 6502 Assembly code. These notes are largely for my own benefit, but those curious about how the sausage is made will certainly get a mouthful.

# Table of Contents
1. [Installation](#installation)
1. [Why The Patch Was Created](#why)
    1. [Background](#background)
    1. [On "Fixing" the game](#fixing)
1. [The Things that were Changed](#them_changes)
    1. [Things Not Changed](#not_changed)

## **Installation**  <a id="installation"></a>
To install the patch, follow these steps:
* Have a copy of the Fantastic Adventure of Dizzy .NES file (*These steps will overwrite the existing file, so make sure to copy the original if you wish to have a vanilla version of the ROM*)
* Download the patch `TheFantasticAdventuresOfDizzy_Fair.ips` from this repository [(link)](https://github.com/schil227/FantasticAdventuresOfDizzyFair/blob/main/TheFantasticAdventuresOfDizzy_Fair.ips).
* Download [Lunar IPS](https://www.romhacking.net/utilities/240/), a program which can be used to apply the patch
* Run Lunar IPS
    * Select "Apply IPS Patch"
    * Locate the TheFantasticAdventuresOfDizzy_Fair.ips file
    * Next, Locate the vanilla Fantastic Adventure of Dizzy .NES file (again, I suggest making a copy beforehand)
* Load the newly-patched ROM (.NES file) into your emulator of choice.

## **Why The Patch Was Created** <a id="why"></a>

This section explains why the patch was created, including some (non-technical) design analysis.

### **Background** <a id="background"></a>
It's amazing just how great FAoD is. The game-play a blend of adventure, platforming, and puzzle, which is really quite impressive. The art-style is crisp and pushes the limits of what the NES was capable of. I'll admit, while play-testing this patch, I caught myself staring at the brick walls of the town sewers; its so fascinating how they achieved the simple illusion of the wall curving as it goes up.

<p align="center">
  <img src="images/FAoDizzy_Fair-Sewer.bmp" alt="Sewers"/><br/>
  <i>Just look at that brickwork!</i>
</p>

And, the music is some of the best in the entire NES library. I occasionally put it on in the background while I'm working - some great bopping tunes. It's a shame that (to put it harshly) they ruined the game with a few terrible choices.

As mentioned, the vanilla version of game is nearly unbeatable. Dizzy takes an absurd amount of damage from everything, and the randomness in enemy movement makes getting hit nearly unavoidable. Grinding the entire game to a halt while Dizzy waits for a spider to get out of his way, only to have it immediately come back down half-way through its ascent, is the epitome of tedium. Not to mention all the 1-hit death-traps, which can be as innocuous as a pale string to a speeding mine-cart collision.

<p align="center">
  <img src="https://cdn.firstwefeast.com/assets/2014/03/7lk1z.gif" alt="Egg getting destroyed"/><br/>
  Granted, they captured the essence of what it's like to be an egg.
</p>

 It's laughable to think of just how much the game is stacked against you. Among imprecise platforming, trial-and-error obstacles, and enemy movement that requires saint-like patience, know that this is all it takes:

<p align="center">
  <img src="images/spider_normal.gif" alt="Egg getting destroyed"/><br/>
  Literally 2 seconds.
</p>

And what happens after you get game over? Back to the title screen. Given all the tasks you must perform and constant backtracking across enemy strewn areas, beating the game is a Herculean task. 

Thus we have the genesis of this patch: how can I minimally change the game such that the extreme difficulty is reduced to something more manageable, while keeping the original spirit of the game intact? In other words, if the original developers realized just how impossible the game was, what would they change?

### **On "Fixing" the game** <a id="fixing"></a>
Claiming that I'm "fixing" the game is certainly a strong statement, one which I do not take lightly. Game design back then was very focused on the player getting their money's worth, and critics tend to throw the phrase "bad game design" around pretty loosely. To the game's credit, yes you certainly would get your money's worth, and yes it really does feel like I'm playing an egg-simulator - however I can not turn a blind eye to some of the more questionable things about this game.

Unlike my Fair Edition patch for [Journey to Silius](https://github.com/schil227/JourneyToSiliusFair), which just skirted over the line of being unreasonable, FAoD stomps all over it. While I was looking at the code and discovered that *just touching* a spider takes away **one fifth** of your total health, I couldn't believe it. I couldn't believe that they would decide to not only throw *dozens* of spiders *just* in the starting, not only that their random movement means you'll spend five to ten seconds waiting for the spider to get all the way out of the way, but that it just lops off a huge chunk of your health. AND due to how i-frames work, you could end up getting hit multiple times! 

... I'm getting off topic. The point is, in this adventure-puzzle-platformer which takes hours to beat, that you could suddenly lose at very swiftly, I felt much more justified in making some changes. And despite my grievances with how some things were handled, I really do think this is a magical game worth the time and effort to "fix". Again, the patch does not aim to trivialize the challenge, but rather to keep the the game as close as possible to its original vision. When the player beats the patch, they should feel like they really completed a tough but (mostly) fair NES game.

## The Things that were Changed <a id="them_changes"></a>

This section will contain an extravaganza of all the many changes that were done as a part of this patch. The details of these changes will stick to game-play and design, and the nitty-gritty coding details will be in the technical section. While the changes for this patch were numerous, it's also worth discussing what *wasn't* changed.

### Things Not Changed <a id="not_changed"></a>

**Continues: No additional continues**

 Like the [Journey to Silius: Fair Edition](https://github.com/schil227/JourneyToSiliusFair) patch, continues (or lack-thereof, in this case) are important in keeping the game challenging. While now-a-days continues are a thing of the past, they undoubtedly served the purpose of keeping the the steaks high and pressure on the player. It really adds to the formula of NES-hard game, and to the elation the player feels once they beat it.

Furthermore, there really isn't a straight-forward way to implement continues that makes sense. Maybe Dizzy could have respawned in his house, but this would've been a very large undertaking for (in my opinion) not much reward. Also had I gone this route, I probably wouldn't have addressed nearly as many of the other issues (e.g. randomness in enemy movement, etc.), since having continues would make the game significantly easier on its own.

**The Minecart Minigame: No Change**

 For what is probably the easiest way to game-over in this game, a small part of me actually enjoys the Minecart Minigame. It really adds a fast-paced splash of variety to the game which (once you 'git gud' at) is pretty great. Even though I had to play through this game like 4 times to test out this patch, every time I beat the minecart section, I felt that lovely dopamine kick.

<p align="center">
  <img src="images/fair_minecart.gif" alt="Minecart clip"/><br/>
  <i>White knuckle baby, YEAH!</i>
</p>

From a technical design standpoint, I don't see a lot of ways to improve it that wouldn't ruin the experience. Make it so you don't lose a life? Remove some of the enemies? Slow down the rifts that sever the tracks? Color-code the "correct" track? Make any exit lead to the end? I don't like any of these options. Fortunately it can be done very early in the game, so I would recommend going straight to it as completing it as possible.

There was a guy I went to college with, his name was Michael. He was a lanky, quite guy who grew up working on his father's farm. He was smart too - pursuing a degree in Mathematics and a minor in Computer Science. Anyways, we wound up working on a group project in class together, and I got to know him. Turns out we both loved the Nintendo 64, and we would arrange to meet at the computer lab after hours to play some Perfect Dark many times. He didn't have many games with him - but one that he did, and seemed to cherish above the others, was his copy of *Star Wars Episode 1: Racer*. I had only a passing awareness of the game, having played it (poorly) at Target and writing it off as to difficult to bother. So one day, while I'm still finishing up some work in the lab, he arrives and he hooks the N64 up to the projector. While waiting for me, he pops in SWE1:Racer and starts playing.

He was *amazing*.

It invoked a certain feeling that I haven't felt in nearly a decade; that feeling of watching your older cousin beat Ocarina of Time. That feeling of being so inept at a task you couldn't being to figure out how to accomplish it - then watching a master do it with ease. Perhaps SWE1:Racer isn't that difficult, maybe I just didn't give it a fair try. But I don't think I ever will, since it would risk tarnishing the nice memory of the best pod-racer I knew.

Anyways - I guess when I play this minecart mini-game, I feel *just a little* like Michael.

**The Clouds**:
<p align="center">
  <img src="images/FAoDizzyFair-Clouds.png" alt="Minecart clip"/><br/>
  His face says it all.
</p>

I'd wager not even the above-average player would make it this far in the vanilla version of the game, but this area sucks. The cloud platforming section is thankfully not a source of many lives lost, but it's arguably one of the most tedious parts of the game. When Dizzy stands on a cloud, he slowly sinks down in it until he falls through it. This means you have to constantly be jumping in place to maintain your altitude. This wouldn't be so bad if not for all the butterflies that threaten to bop you around and reverse your controls. What's more, you have to do this section several times.

All that being said, I could have easily changed it. I could have made Dizzy sink in the clouds at a slower speed. I could have removed some of the butterflies. But this is one of the very few parts of the game that doesn't immediately bring death and dismemberment to Dizzy. To me, it serves as a reminder; a reflecting pool before the final boss - a reminder to just how tedious this game was before the patch. Therefore, it stays in unaltered.

**The Balls in Zak's Castle: No Change**

 Without spoiling the ending any further, it would be disingenuous of me to alter any part of the final encounter. That last area is pure FAoD.

### Changes to Dizzy
These are the changes that were made to Dizzy directly

**Starting Lives: Dizzy starts with one extra life**

 I went back in forth on keeping this change in, but with how quickly lives can get snapped up, I think it's fairly benign. If anything it's a reminder to the quality assurance that went into this patch: before this I had instead opted to double the lives gained when completing the sliding puzzle mini-game. However when I ended up beating the game with like five extra lives, I figured it was ruining the economy. So it stands, one extra life.

**Invincibility Frames: Invincibility frames are doubled**

 Due to how damage works in this game, I was kinda surprised to find that Dizzy has invincibility frames (i-frames). The way it works is Dizzy get hit, for, say 10 damage. Then, Dizzy would be invulnerable to damage for 10 frames. Having a one to one ratio of damage taken vs i-frame is pretty sorry, and the result is Dizzy will typically be hit multiple times if he touches an enemy. To add to this point, since i-frames are tied to damage received, decreasing the damage enemies do makes almost no difference. For example, if Dizzy takes 5 damage, he only has 5 frames to get out of harms way.

The solution was to double the i-frames Dizzy receives after getting hit. This was done in conjunction with reducing the damage from many different sources. These two changes alone made for a much more enjoyable experience; as opposed to the ultra-aggressive-two-seconds-until-you-are-dead experience.

### Changes to Enemies
**Spiders: Reduced damage and removed switchbacks**

| <p style="text-align: center;">Base Game</p> | <p style="text-align: center;">Fair Edition </p> |
|---|---|
| <img src="images/base_spider.gif" style="width:342px" alt="fair game spider movement"/><br/>|<img src="images/fair_spider.gif" style="width:342px" alt="base game spider movement"/> |
| <p style="text-align: center;"> Spiders move erratically</p> | <p style="text-align: center;"> Spiders move consistently up and down</p>|

Many enemies in the game move in a random way which I'll refer to as "switchbacks". Switchbacks happen when the enemy is seemingly moving in one direction, but then "switches back" part way through. This is most obviously seen in the spiders from the base game: the spider moves down quickly, goes up a little, but then does a fake-out and drops all the way down half way through. Dizzy does not move fast enough to dodge the spider unless it is at the very top, which means you need iron patience to *always* wait *every time* for the spider to go all the way up in the base game, or run the risk and lose 20% of your health.

This change is worth the patch alone. A large portion of the game take place in spider-infested tree labyrinth, and it's easily one of the most frustrating parts of the game. This change means that if a spider is going up, it's going up. It's not going to switch and drop halfway through, no, it's just going up. All the way up. And that's wonderful.

In addition, spider damage was reduced by about 2/3rds, meaning it takes roughly 13 hits from the spider to kill Dizzy, as opposed to the usual 5.

**Birds: Reduced damage and removed switchbacks**
| <p style="text-align: center;">Base Game</p> | <p style="text-align: center;">Fair Edition </p> |
|---|---|
| <img src="images/base_bird.gif" style="width:342px" /><br/>|<img src="images/fair_bird.gif" style="width:342px" /> |
| <p style="text-align: center;"> Birds change direction frequently</p> | <p style="text-align: center;"> Birds only change direction when they hit an edge</p>|

If it's not the spiders, then it's the birds (and bats). These bastards change direction and speed frequently mid-flight, *and* they ignore i-frames! Meaning it's more or less random chance if passing by one will peck your yolk out.

Since they ignore i-frames, I reduced the damage from two per frame to one. And similar to the spiders, I removed the switch-back logic. In this case, the birds only switch direction when they bounce against the edges of their area.

<p align="center">
  <img src="https://thumbs.gfycat.com/HandsomeDeafeningAmethystgemclam-max-1mb.gif" alt="DVD screensaver"/><br/>
  Big screensaver vibes
</p>

I was surprised by how effective this was; being able to see the consistent pattern made it so there was some actual game-play involved, instead of just praying not to get hit.

**Sewer Rats: Reduced damage and removed switchbacks**
| <p style="text-align: center;">Base Game</p> | <p style="text-align: center;">Fair Edition </p> |
|---|---|
| <img src="images/base_mice.gif" style="width:342px" /><br/>|<img src="images/fair_mice.gif" style="width:342px" /> |
| <p style="text-align: center;"> Mice change direction frequently</p> | <p style="text-align: center;"> Mice only change direction when they hit an edge</p>|

Essentially the same problem as the birds, except it's harder to tell if they're hitting you. The solution is essentially the same as well; they only change direction when they hit the wall, and their damage has been reduced.

**Guillotine: Removed switchbacks**
| <p style="text-align: center;">Base Game</p> | <p style="text-align: center;">Fair Edition </p> |
|---|---|
| <img src="images/base_guillotine.gif" style="width:342px" /><br/>|<img src="images/fair_guillotine.gif" style="width:342px" /> |
| <p style="text-align: center;"> Guillotine can drop at any time</p> | <p style="text-align: center;"> Guillotine only drops when it's at the top</p>|

Any executioner worth their salt would know that if you want to effectively kill someone, you shouldn't half-ass it. The same could be said for the rather uncomfortably common placement of Guillotines throughout the town. Instead of the frequent occurrence of misfires, now the Guillotines must reach the top before coming down. Of course, if Dizzy gets hit by one he'll still be killed - but at least you can time your movements to escape such a fate, if you're careful.

**And The Rest: Reduced damage from several other sources**

These sources include:
  * **Triceratops**: Reduced from 25 points of damage to 16
  * **Sheamus the Leprechaun**: For some reason he did a ton of damage. I reduced it to be a bit more fair, since it's not obvious he'll hurt you. Reduced from 25 points of damage to 5
  * **Fire Torches**: Also ignores i-frames, reduced to 1 point of damage per frame.
  * **Lava Rock**: Ignores i-frames, reduced from 5 points to 2 points per frame.
  * **Rats**: As in the rats not found in the sewers. Their damage was reduced from 20 to 7.
  * **Ants**: To be fair, they're quite large. Their damage was reduced from 20 to 7. 

<p align="center">
  <img src="images/FAoDizzy_Tri.png" alt="Triceratops "/><br/>
  Strange looking dog.
</p>

### Environmental Changes

Outside of Dizzy and the things which wish him harm, there were several more subtle changes that were introduced as well. These include:

**Barrel Over the Falls: Increased time to jump on the barrel**
| <p style="text-align: center;">Base Game</p> | <p style="text-align: center;">Fair Edition </p> |
|---|---|
| <img src="images/base_barrel.gif" style="width:342px" /><br/>|<img src="images/fair_barrel.gif" style="width:342px" /> |
| <p style="text-align: center;"> Very brief window to jump on the barrel</p> | <p style="text-align: center;"> Slightly more generous window to jump on the barrel </p>|

Even today when I encounter this jump I get a slight zap of anxiety. Given how many times you have to cross it, adding even just a few more frames makes all the difference.

**Bubble Mini-game: Slowed down how quickly oxygen decreases**
| <p style="text-align: center;">Base Game</p> | <p style="text-align: center;">Fair Edition </p> |
|---|---|
| <img src="images/base_oxygen.gif" style="width:342px" /><br/>|<img src="images/fair_oxygen.gif" style="width:342px" /> |
| <p style="text-align: center;"> Oxygen meter ticks down somewhat quickly</p> | <p style="text-align: center;"> Oxygen meter ticks down slightly slower</p>|

Once you master the bubble mini-game, it's not to terribly difficult. But I guarantee the first few times you play it, Dizzy will almost certainly drown. It's a pretty cool mini-game, but a lot of mechanics are thrown at you. If you don't catch on immediately (and get lucky, frankly), it's a watery grave for Dizzy. 

In order to round out the difficulty (and upset the luck-factor), Dizzy's oxygen meter decreases 1/3 slower in the fair edition than the base game. This gives the player ample time to figure out what's going on and how the new mechanics work.

**Graveyard Cavern: Removed particularly awful raindrop**
| <p style="text-align: center;">Base Game</p> | <p style="text-align: center;">Fair Edition </p> |
|---|---|
| <img src="images/base_raindrop.gif" style="width:342px" /><br/>|<img src="images/fair_raindrop.gif" style="width:342px" /> |
| <p style="text-align: center;"> Oxygen meter ticks down somewhat quickly</p> | <p style="text-align: center;"> Oxygen meter ticks down slightly slower</p>|

At the very end of the game, one of the last items you get is a trampoline - and after playing through the game four times, I can still say with confidence that I have no idea how the thing is supposed to work. But its purpose is to bounce dizzy out of a cavern, and at the only spot where you can do that is a god damn raindrop. Why - why put a raindrop *there* of all places? Why punish the player attempting to learn a new, *very obtuse* mechanic at the end of the game?

 I actually lost a life at that part, and I just couldn't stand the thought of someone getting game over there, after overcoming all the trials of the game. I resisted the urge to just remove enemies and hazards for the entire game, but this raindrop was a bridge too far.

 ### Bug Fixes

 There are a few 