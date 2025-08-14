+++
title = 'Playing Screeps - My first day'
date = 2025-08-13T00:00:00Z
draft = true
type = 'post'
+++

# What is Screeps?

[Screeps](https://screeps.com/) is an MMO sandbox game for programmers released 16th November 2016 with the last update seemingly 8th November 2021. The overall goal is to write Javascript to guide your creeps on how to expand your colony through gathering of resources, building of structures and capturing of "rooms". This can involve Creep Vs. Creep action, so the PVP aspect is covered.

## My Approach

I am not a Javascript guy. I repeat I am not a Javascript guy, but I have dabbled in Python, GoLang, Rust, and just a bit of Javascript and Typescript in the past.

So I figure this will be a fun learning exercise. The goal is no LLMs beyond playing tutor to help me fix my JS bugs - I see real value here in speeding up my learning but will not be reaching out immediately to help reinforce the learning. LLMs make my brain incredibly lazy.

Originally I wanted to get playing with TypeScript which I think will be of benefit to me more in my job but for now I wanted to jump right into the game with Javascript and not spend all day playing with VS code for something I may not enjoy.

## Screeps Day 0

I played through the tutorial which covers the very basics of spawning creeps, gathering resources and building. Then opened training mode and started trying to learn the game itself, so many tutorials out there focus on the code which seems for many to be a case of copy pasting (boring). This seems to be the hardest things for Screeps, there is no clear documentation of what I should be building towards, for what purpose the market exists, just exactly what a Nuke does and what it takes to spawn a PowerCreep.

Some things I read about in the [API docs](https://docs.screeps.com/api/) that will likely be useful one day in the future if I continue playing and don't get wiped out immediately but currently do not understand:

- Nukes
- Power Creeps
- Labs
- Factories
- Terminals
- Observers

Others sound like they make sense such as Portals, InvaderCores, KeeperLairs but honestly I don't know what these are either and there's a good few more things I have likely overlooked in the docs. All this to say, the game seems to be plenty complex enough that I doubt anyone could truly master it but no doubt in the nearly 9 years that the game has been released there are some incredibly advanced solutions.

So from reading the docs I decided I'd play the game blind and just start trying to build my own code from scratch, no references to how others are playing beyond what I can learn in-game itself and no using of any OSS.

First, I took what little code I had from the tutorial and started iterating, this didn't feel like cheating in the same way OSS / an LLM would be.

### Priority One - Beyond Survival

To survive in Screeps you have to harvest power from a Source, upgrade your Controller and fend off any attacks. The game is broken into rooms which you want to conquer as many of as possible, at least I think that's the case anyway.

Beyond this you can spawn more creeps (within a CPU / memory limit) and add to your workforce.

You want fewer "bigger" creeps that have many parts (costing more power) so that you can achieve more within your limited CPU and Memory constraints - though I have yet to go anywhere near these because I have achieved so little so far.

#### Multi Source Mining

I deployed the training room and got to work, my harvesters all went to a single Source to gather energy - Seems like an obvious immediate bottleneck.

So the logic I followed became simple, when a creep determines that it needs to move to a source an assessment of how busy that source is is made. I defined a radius around the source and then iterate over the position of all other creeps relative to the position of the source, if there are more than X number of creeps within that radius then they will choose another source.

It worked! Almost. The problem was that every time the main loop runs if a harvester is not yet within range of a source then they would rerun the determineSource function and if the source was no longer busy then they would start heading towards the first source. When the source gets busy again they flipflopped back to the secondary source thus getting stuck between the two sources and harvesting nothing.

To manage this turned out to be very simple, save in the memory of each creep the choice of source, if this value is set then we exit the selection function early and we are off to the races! Mining from multiple sources successfully.

#### Managing Spawns

To achieve an automated solution you do not want to have to manually do anything really, ideally the game is entirely played by your code. This extends to the creation of new creeps, something that you do during the tutorial however it was quite basic as such this was the next thing I wanted further control over.

I had created a `constants.js` file to manage what would otherwise be a series of magic numbers throughout my codebase. Within this I created objects for each role of creep that I would have within my colony and parameters for each of them including the attributes with which they should spawn, the role name and a total number of them that the colony should maintain before ceasing to produce any further.

This allows for a single file to be changed if I notice that the colony is misbehaving in some easily correctable manner.

An order of priority was created such that harvesters, then builders and then upgraders would be spawned and this seemed to work well.

#### Good Enough!

I'd like to try jumping straight into the game from here, I threw down some buildings and they build but I don't believe I will be able to learn enough from the training room so day 1 will start with my diving straight into the MMO aspects of this intriguing little game... right after finishing up this blog post just now.

## What Javascript did I learn today?

In general this was a refreshed in Javascript syntax but coming from other languages it wasn't hard to just start running with by simply reading the API docs of the game and a little trial and error within the console.

Remembering to throw `console.log()` everywhere when my code misbehaved usually due to a bad returned value was paramount for my personal debugging, Python's equivalent of `print()`.

### On Loops

Apparently you cannot use a `break;` or `return` statement when iterating over an array using the .forEach() method. Why? Well according to [this article](https://dev.to/codenutt/why-you-can-t-break-a-foreach-loop-bytesize-js-d51) it would seem that it is because it is running a callback function over each item, as such you are calling break in a function that is not aware that it is part of a loop (for break) and for the case of return it is just returning within that callback function. Annoying but whatever, I can see why that is a thing.

`for ( const thing in things ) {};` when thing was an object I was getting a damn String of value `'0'` returned - excuse me? Well apparently it's not just Javascript but other languages as well, you have to use a `for (const thing of things) {};` to access the object and its attributes.

### On NodeJS

How to import between files, it's just simply not something I've needed to do in the past but I like separating my code apart. This is something I think I want to improve as right now my solution is:

`var roleHarvester = require('role.harvester');`

From there I make references to the functions available in the module by calling them e.g. `roleHarvester.run(creep)` and it works well enough for now. In my head I would rather import the exact functions I want from the module rather than the entire thing.
