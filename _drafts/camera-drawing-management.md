---
title: Perspective Camera Tip #1, Manage the Drawing Yourself
date: 2025-05-13 16:00
categories: [Plush Rangers, Devlog]
tags: [gamedev, gamemaker, perspective-camera]     # TAG names should always be lowercase
description: Some easy tips to help make drawing easier. Works for all styles, but is essential for working with perspective cameras.
---

### What could go wrong?

When I started building out the perspective camera for Plush Rangers, I used layers to try and achieve some control on where different items were drawn. I had a "Ground Layer" that had rocks and things sitting on the ground, an "Actors" layer for things that were animated, an "Effects" layer for any particles and things and it worked... ok. 

When I'm working with general 2D games, I love the layer system. It's intuitive and it's easy to understand how it will interact with your objects. Things drawn in later layers are drawn on top.

Perspective cameras break that paradigm. Things drawn in later layers can technically be behind other things drawn in previous layers. The layers don't mean the same thing anymore. 

Once you turn on z-buffer reading/writing with transparency and blending. Things really start to break down. You need to take more active control of your entities and the layer system becomes more effort than value at this point.

### You are in control

