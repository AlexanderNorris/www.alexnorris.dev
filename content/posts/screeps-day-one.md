+++
title = 'Playing Screeps - My first day(s)'
date = 2023-10-31T07:53:28Z
draft = true
type = 'post'
+++

# What is Screeps?

[Screeps|https://screeps.com/] is an MMO sandbox game for programmers released 16th November 2016 with the last update seemingly 8th November 2021. The overall goal is to write Javascript to guide your creeps on how to expand your colony through gathering of resources, building of structures and capturing of "rooms". This can involve Creep Vs. Creep action, so the PVP aspect is covered.

## My Approach

I am not a Javascript guy. I repeat I am not a Javascript guy, but I have dabbled in Python, GoLang, Rust, and just a bit of Javascript and Typescript in the past.

So I figure this will be a fun learning exercise. The goal is no LLMs beyond playing tutor to help me fix my JS bugs - I see real value here and will not be reaching out to them immediately to help reinforce the learning.

Originally I wanted to get playing with TypeScript which I think will be of benefit to me more in my job but for now I wanted to jump right into the game with Javascript and not spend all day playing with VS code for something I may not enjoy.

## Screeps Day 0

I played through the tutorial which covers the very basics of spawning creeps, gathering resources and building. Then opened training mode and started trying to learn the game itself, so many tutorials out there focus on the code which seems for many to be a case of copy pasting (boring). This seems to be the hardest things for Screeps, there is no clear documentation of what I should be building towards, for what purpose the market exists, just exactly what a Nuke does and what it takes to spawn a PowerCreep.

Some things I read about in the [API docs|https://docs.screeps.com/api/] that will likely be useful one day in the future if I continue playing and don't get wiped out immediately but currently do not understand:

- Nukes
- Power Creeps
- Labs
- Factories
- Terminals
- Observers

Others sound like they make sense such as Portals, InvaderCores, KeeperLairs but honestly I don't know what these are either and there's a good few more things I have likely overlooked in the docs. All this to say, the game seems to be plenty complex enough that I doubt anyone could truly master it but no doubt in the nearly 9 years that the game has been released there are some incredibly advanced solutions.

So from reading the docs I decided I'd play the game blind and just start trying to build my own code from scratch, no references to how others are playing beyond what I can learn in-game itself and no using of any OSS.

First, I took what little code I had from the tutorial and started iterating. Priority one was to manage my colony and get it building

## What Javascript did I learn today?

On Loops:

Apparently you cannot use a `break;` or `return` statement when iterating over an array using the .forEach() method. Why? Well according to [this article|https://dev.to/codenutt/why-you-can-t-break-a-foreach-loop-bytesize-js-d51] it would seem that it is because it is running a callback function over each item, as such you are calling break in a function that is not aware that it is part of a loop (for break) and for the case of return it is just returning within that callback function. Annoying but whatever, I can see why that is a thing.

`for ( const thing in things ) {};` when thing was an object I was getting a damn String of value `'0'` returned - excuse me? Well apparently it's not just Javascript but other languages as well, you have to use a `for (const thing of things) {};` to access the object and its attributes.

On NodeJS:

How to import between files, it's just simply not something I've needed to do in the past but I like separating my code apart. This is something I think I want to improve as right now my solution is:
`var roleHarvester = require('role.harvester');`
From there I make references to the functions available in the module by calling them e.g. `roleHarvester.run(creep)` and it works well enough for now.
In my head I would rather import the exact functions I want from the module rather than the entire thing.
