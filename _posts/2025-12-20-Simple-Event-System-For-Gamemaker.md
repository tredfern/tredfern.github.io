---
title: Super Simple Eventing System in Gamemaker
date: 2025-12-20 09:00
categories: [Plush Rangers, Devlog]
tags: [gamedev, gamemaker]     # TAG names should always be lowercase
description: Allow your objects to broadcast messages without worrying about where they are going.
---

## Overview
[Plush Rangers](https://store.steampowered.com/app/3593330/Plush_Rangers/) has lots of enemies, projectiles, characters, powerups, etc... that need to have some custom behaviors to occur at certain times. An example is that there are pillars that are scattered around the map that need to know when you kill an enemy within a certain area. Trying to link this all together directly would start to introduce a lot of tightly coupled code that would be difficult to change and maintain. Also, it does nothing to help add even more behaviors or situations. 

To solve this I came up with a simple lightweight event system using tags that allows for objects to subscribe to events without any centralized publisher/subscriber system. This makes it really simple to add events and loosely link objects together to add more behaviors. 

### Code

The code itself is quite simple, just get the tags, and call the objects.

<pre>
/// @desc Trigger any event handlers on objects for specified event
/// @param {String} _event Name of event to trigger
/// @param {Id.Instance} _source Originator of the event
/// @param {Struct} _args Any additional data to be passed with the vent
function triggerTaggedObjectEvent(_event, _source = id, _args = {}) {
    // Find all assets tagged with event
    var _tagged = tag_get_asset_ids(_event, asset_object);
    
    // Loop through tagged objects
    for (var _i = 0; _i < array_length(_tagged); _i++) {
        with (_tagged[_i]) {
            // Ensure we actually have a handler defined
            if(variable_instance_exists(id, _event)) {
                // Call the event handler
                self[$ _event](_source, _args);
            }
        }
    }
}
</pre>

### Usage

1. Add tags to objects that subscribe to a specific event. I use a format like `onEnemyDestroyed` or `onLevelUp` to help make it clear the tags are for handling events.
2. Inside the tagged object, add the event handler function named the same as the tag: `onEnemyDestroyed = function(_source, _args) { ... }`
3. Where the event occurs (in this example the obj_enemy destroy event), call the trigger script passing arguments: `triggerTaggedObjectEvent("onEnemyDestroy", id, { killedby: player_1 });`
4. Enjoy having simple events!

### Pros/Cons

I have found this system as a very simple lightweight way to link objects and events together without creating a lot of spaghetti. It makes it easy to change behaviors, add new ones, and avoid having to spend a lot of time figuring out how X needs to call Y. 

Events can also be raised by anything that is running code. A user interface button, a struct, some state machine, etc... Anything can call the function and trigger events to the handlers.

It also avoids having to write any custom subscription code. The subscriptions are defined by the tag system. You can then use the asset browser to filter for tags to find objects that subscribe to a particular event.

Unfortunately, using tags means that only instances of objects can actually handle events. Structs cannot directly handle events themselves. A centralized instance that handles the events to call the structs is an option if necessary. 

Performance wise, I haven't worried about it. Usually this isn't for code that is happening every tick. 

Hope this helps!
