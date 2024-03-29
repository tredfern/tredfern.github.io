---
layout: page
title: The Terminus Project
permalink: /terminus/
---

[![Build Status](https://travis-ci.com/tredfern/terminus.svg?branch=main)](https://travis-ci.com/tredfern/terminus)
[![Coverage Status](https://coveralls.io/repos/github/tredfern/terminus/badge.svg?branch=main)](https://coveralls.io/github/tredfern/terminus?branch=main)

> ### The Beginning
> After a tiring 2020, I decided to do something different for myself in 2021. Each weekend, I will be streaming 
> live development for 3-4 hours on this project and talking about what steps I'm taking. I will provide insights 
> into how to write clean tests, different ways to solve programming challenges, and at some point, 
> maybe even have an interesting product.
> This is not a tutorial on how to approach the project. My hope is that streaming a non-tutorial project will help demonstrate solving challenges that tutorials tend to avoid which such as:
> * How to approach *Test Driven Development* and use it to write better code
> * When and how to *refactor*
> * How to *evaluate solutions* when multiple are possible
> * When to write *scaffolding/temporary code* and when to invest in your architecture

[![Build Status](https://travis-ci.com/tredfern/terminus.svg?branch=main)](https://travis-ci.com/tredfern/terminus)
[![Coverage Status](https://coveralls.io/repos/github/tredfern/terminus/badge.svg?branch=main)](https://coveralls.io/github/tredfern/terminus?branch=main)
[![GitHub milestone](https://img.shields.io/github/milestones/progress-percent/tredfern/terminus/3)](https://github.com/tredfern/terminus/milestone/3)
[![Love2D Version](https://img.shields.io/badge/love2d-11.3-pink.svg)](https://love2d.org)

# The Terminus Project

Develop a game that combines my _learnings of systems thinking_, _opinions on software engineering_, and _love for games that give control to the player to influence the world_.

## Goals
### Fun
The game should be fun to play. That fun should come from the combination of challenge and discovery.

### 99% Unit Test Coverage
A strange goal perhaps. I believe that TDD can assist any project. Because of the planned complexity of interactions between entities, unit tests will provide a foundation to validate those interactions and potentially test balance.

### Easy to Mod
The game should feel like a framework that could be extended. Similar to creating an RPG system where the adventures, items and characters can be created by anyone.


### High Replay Value
The game should have either different challenges or feel so different every time that it encourages people to play again.


### A Feeling of Story
The game should have a feeling that a story is occurring and the player is contributing to the creation of that story.


## Development

[**Weekly Blog Posts**](https://wheretheredfern.code) giving a brief overview of the code changes implemented and screenshots of progress.

[**Live Coding on Twitch**](https://twitch.tv/wheretheredferncodes)
Saturdays & Sundays 13:00-15:00 EST.

[**Milestone Progress**](https://github.com/tredfern/terminus/milestones) will track significant changes and progress.

[**Builds / Releases**](https://github.com/tredfern/terminus/releases) will be automatically created for every tagged push.

## Concept

An RPG based game/system set in a sci-fi universe. Player is one of a small group of soldiers that
awakens on a ship/space station/moon base to a major disaster in progress. Player needs to navigate rogue robots and 
damaged systems to repair the ship to save the rest of the crew. 

Play styles could vary between more combat expertise, which will increase survivability but might limit exploration
or other skill opportunities. Or maybe using more survival skills which will help with finding supplies and
alternate routes. Or maybe a more skill focus that allows the character more abilities to explore and solve problems.

## Theme / Setting

_Survival Theme: The hero desire to keep taking a step forward even when facing overwhelming odds just to survive._

Potential deeper themes:
* When society is collapsing, what is the balance between preserving ourselves and preserving the people around us?
* Turn-based game that allows the player time to think and choose actions
* However, action-based game would allow the chaos of the situation to feel more present

## Inspiration

I'm focusing my inspiration on games from the past. Certainly there will be ideas that will leverage current games including things like Dwarf Fortress/Rimworld or Caves of Qud, or Fall Guys (inspiration can and should come from anywhere). These old games captured my mind when I started playing them and finding what made that spark happen is what I'm curious about exploring with this project.

**[The Magic Candle](https://en.wikipedia.org/wiki/The_Magic_Candle)** - I played this game when it first came out in 1989. It has been a while since I last played it, but I remember being impressed by the mechanics of exploration. This game felt huge. And also the ways that you managed your party. 

**[Darklands](https://en.wikipedia.org/wiki/Darklands_%28video_game%29)** - A very interesting RPG that allowed a lot of freedom in how you approached the game. But one of my favorite features was that you didn't have to move your character through each individual square when walking through a town. How much time did I waste in most RPG's just clicking from one side of the map to another just to complete a quest? Being able to navigate to locations was fantastic.

**[Ultima V](https://en.wikipedia.org/wiki/Ultima_V%3A_Warriors_of_Destiny)** - The main Ultima game I played. I loved the variety of towns and quests that linked things together. Getting the magic carpet, was awesome. 

**[Everything Epyx](https://en.wikipedia.org/wiki/Epyx)** - I grew up looking at everything Epyx released. I loved their motto even as a kid of combining strategy with action. Some of their games that are influencing my thinking on this project are, Temple of Apshai Trilogy, Dragonriders of Pern (I'm serious, I played that game a ton),

An interesting tidbit I learned from research is that Epyx did the port of Rogue to non-Unix platforms.

**[Force 7](https://www.mobygames.com/game/c64/force-7)** - There was this game on the C64 where you had a crew and needed to eliminate all these aliens in various rooms.

**[Cogmind](https://www.gridsagegames.com/cogmind/)** - similar genre and also sci-fi. Different play styles available and need to keep alert level down.

**[Dungeon of the Endless](https://www.mergegames.com/dungeon-of-the-endless)** - Random generated dungeon of rooms where you are trying to get to the top level

**[Rimworld](https://rimworldgame.com/)** - If the game evolves into more of a base-building mechanic

**[Shadowgrounds](https://shadowgroundsgame.com/)** - Aliens attacking a base and working through levels to solve/protect it

**[Metamorphisis Alpha](https://www.drivethrurpg.com/product/58985/Metamorphosis-Alpha-4th-Edition)** - Never played this, but the general story is similar and a strong basis for inspiration.

**[Traveller](https://en.wikipedia.org/wiki/Traveller_%28role-playing_game%29)** - Never played this, character design is interesting. Generally seen as a very open-ended RPG system in a sci-fi world that can help inspire how to face certain design decisions for the underlying mechanics.

#### Books / Movies
_Orphans in the Sky_ - I read this book as a child and remember it still. 

_Aliens_ - Both from Newt's survival session on her own to the marines fighting their way in

_Diehard_ - Facing off against an invading force on your own. Identifying your enemy and their intentions

_Lost in Space_ - Traveling to a new world, a catastrophe happens and people band together to survive

## Key Milestones
### 0. Basic foundations [COMPLETED]
This is some next steps to lay good foundations depending on what comes up next
  - Load/Save game: Always a tricky thing to work out, building this in early should be easier than deferring to the last moment. Use a save game slot system
  - Options screen: Simple ability to set resolution for game, change key maps. This is to prevent anything being too rigid to change later
  - Character Details Screen: Something in game to pop-up a display over the map

### 1. Simple never ending roguelike [COMPLETED]
This should focus on moving the character around an testing the combat system
  - Inventory to equip melee and ranged weapon
  - Create enemy spawners that trigger new enemies whenever rooms are empty
  - Combat has some strategic feel
    - Ranged Combat
    - Melee Combat
  - Character generation gives some ability scores and combat skills
  - AI that charges the player down
  - Some ability to heal character

### 2. Foundational Improvements <-- I AM HERE -->
- Multilevel Maps
- Animation System
- Better UI/UX building blocks
- Better entity management
- Better location management
- Better information for AI/Map decisions

### 3. Milestone 3 - Characters and BestiaryoMore Monsters
Add NPCs
Improve Character Creation
New AI functions
  - Patrolling AI that moves from room to room 
  - Guard AI that stays in room until the player enters
  - Repair AI that fixes up enemy units that are damaged
  - Fleeing AI that runs after taking a certain amount of damage
  - Introduce Allies that assist player
    - AI Holding rooms that are cleared
    - AI Follow player and assist in battle

### 4. Story Events
  - Introduce quests
  - Messages that can happen describing what is going on
  - Intro screen setting up the scene
  - Define a story arc based on the levels that you move through
  - Introduce different environment conditions (Fire and Vacuum)

### 5. Equipment, Items, Crafting
- Expand inventory of items
- Expand map features and items that can be interacted with
- What kinds of crafting, customization should be available?
- Build out UI/UX for interacting with items.

### 6. Feedback Cycle
Get an initial round of feedback and address obvious shortcomings

### 7. Audio/Visual Enhancements
A milestone to focus on improving the visual components for the game

### 8. Environment and Hazards
Fire, Vacuum, Fluids, Gases, etc... Those things that make the environment of the world feel alive (or deadly)

### 9. Initial Release

## Technical Notes

### Framework

This project depends on the [Moonpie](https://github.com/tredfern/moonpie) framework for [Love2D](https://love2d.org).
This framework is designed to organize code into clear separation of responsibilities. This allows for the ability
to create unit tests efficiently for any point of the system that can benefit from testing.

The key pieces of the framework are the `store` and `ui.components`. The `ui.components` provide an easy way to define
the UI in a responsive way. Making it easy to layout the style and various controls necessary for displaying and
receiving user input. The `store` manages the game state and all the information happening in the game.

Both of these patterns are similar in spirit to [React-Redux](https://react-redux.js.org/). By isolating state and
making sure all updates are coordinated through actions and reducers, we can easily control state changes and centralize
the updates.