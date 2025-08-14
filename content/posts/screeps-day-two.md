+++
title = 'Playing Screeps - Day Two'
date = 2025-08-14T09:00:00Z
draft = false
type = 'post'
+++

# Humble Beginnings

The thing about Screeps is that I don't really know "how to play" I have spawned in the main game in a room with two sources and some utrium in the Newbie section meaning that I should be safe for a while and have some resources to mine.

As with many things in my life I enjoy having a goal, so for now my goal is to get my controller to level 3 so that I can build extensions and towers.

## How does my initial code do?

Immediately a harvester pops out thanks to my priority order which seems to have at least that right. He goes and harvests bringing back enough Energy to build another and we are off to the races when that one pops.

I've specified an initial population of 10 harvesters, 5 builders, and 10 upgraders. Turns out however now the game's limitations that didn't exist in the training room are starting to kick in. With just two creeps my no doubt incredibly inefficient code is already consuming 1 CPU! So rethinking this I have reduced this down to 10 harvesters, 2 builders and 5 upgraders. We can see where the CPU utilisation goes from there.

There was some initial crowding that took place prior to the second spawn being mined due to selection of sources occurring prior to a long walk, when the creeps were all bunched together that meant every creep travelling at the same time would check when there were no other creeps nearby.

This seemed to work itself out nicely and eventually the second spawn began being used. I almost wonder if just a random selection would work better for efficiencies sake or assinging specific harvesters to specific sources. This can be a future consideration for improvement.

Encountered bug revealing that only harvesters were choosing sources correctly. [Fixed]

Maybe not efficient, but resilient which is perhaps better. Noticing that my creeps start avoiding _busy_ sources I am quite pleased as running for a long time without issue is likely incredibly valuable.

... Time passes...

I got a rank!!! It might not be much but I made it to rank 1299 out of 1312 🙂

You can see the ranks for Screeps over [here](https://screeps.com/a/#!/rank/world/2025-08) and if you search my name `NorNor` you will likely find it... somewhere... I managed to grab my first rank and I am not the lowest scorer! Forgive the low standards but I had no idea how hard this game would be for someone clueless about Javascript, I am proud of my little colony.

Not only is this really cool but after a few brief moments I refreshed the page and I am up one more rank. The game is starting to get fun as my creeps can get on without me, but I have not get got to the any kind of depth in the game. I am still using basic creeps and trying to get off the ground to level up to a point whereby I can afford and build a single turret.

I should in theory accomplish my first goal shortly. So at this point I will reassess, my new goal should be a rank of under 1k so I can say that I am officially one of the top 1000 players in the world - we will ignore the fact that there are only 1312 of us this month 😉

## Next Steps

OK I am starting to get into this, I definitely cannot see the big picture yet but there is a small picture I can see and that is right to the South of my current base.

### More Energy

A juicy extra Source of energy with no one else mining it. I need to be out of the house in around about one hour and then this whole experiment will be running for more than 12 hours without any intervention from myself so let's try to hack something simple and dirty.

Reading the [moveTo](https://docs.screeps.com/api/#Creep.moveTo) API docs I find that there is a Boolean option for noPathFinding which explicitly states that it will save CPU. Noted for the future.

From the docs I can see it appears that I can simply instruct my creeps to move to another room's coordinates using to the moveTo function whilst providing the room name `creep.moveTo(new RoomPosition(25, 20, 'W10N5'));` how to get that room name.... well I couldn't find it in the docs but with a little help from the console `creep = Game.creeps['upgrader72236929'].room.name;` is my current room

With some tinkering we have a working second room being mined! And with a little bit more tinkering, the creep goes home to the spawner.

Now I need to remember to spawn these remote mining boys, I give them a new role, `long_harvester`, this is purely so that my spawner logic knows that they should be counted separately from how many `harvester`(s) I have. I just add an or statement so that when calling the harvester function it is called as such:

```
if (creep.memory.role === 'harvester' || creep.memory.role === 'long_harvester') {
            roleHarvester.run(creep);
}
```

## World Observations

Looking how others are playing within the newbie section of the game I make the following observations:

### Player #1 - alkom1 (769,635)

- Looks like a Sims logo straight East.
- This first individual has 9 creeps total.
- Their creeps are by enlarge made of 10 to 12 parts.
- Their creeps are not (yet?) mining the keanium that is in their room.
- They have specialised WORKx5,MOVEx5 harvesters who seem to drop(?) the energy to CARRYx5,MOVEx5 creeps.

### Player #2 - Mirroar (75,139,745)

- Straight South, red looking peace symbol flag states "Room managed by hivemind" and links off to a GitHub page.
- This individual is #77 on the scoreboard
- Their creeps have 50 parts 😵‍💫
- They use ramparts to wall themselves in which seem to have dizzying hitpoints of 300M
- They are likely too far advanced to draw anything more meaningful from.

### Player #3 - MrMarvel (9,740)

- Cool looking nuclear symbol logo to my North East
- Looks like someone else straight out of the tutorial! Thrilled to see people playing the game like me.
- 11 creeps total all with a default WORKx1,MOVEx1,CARRYx1 body

## Screeps Gameplay Thoughts

So the Scoreboard makes it seem like the overall goal of the game is to upgrade your Controllers.

Creeps can get massive at 50 body parts each! That's a big boy.

The training room didn't have any hints as to CPU utilisation. It would seem that having twenty creeps in total would be a large number. Or my code is insanely inefficient already which seems unlikely due to it doing so very little. Perhaps the pathfinding is the costly effort. In which case just setting individual harvesters to specific sources would seemingly be incredibly efficient.

## Possible Improvements

At this point I am starting to enjoy the code and can see endless possibilities.

- Add more buildings and roads.
- Modularise the code so that it is more readable.
- Add an upgrader so that my creeps are dynamically upgraded when possible.
- Create more bespoke creeps with new logic.
- Programattic building placement.

## Fun

Not everything is about scoring more, I think it would be great fun to learn how to do some of the following:

- PvP
- Trading with NPCs? - For what purpose I don't know still but I read it is possible in the API docs
