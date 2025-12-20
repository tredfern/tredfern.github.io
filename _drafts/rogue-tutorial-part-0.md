This is a traditional roguelike tutorial written for Gamemaker that will follow the format and structure of the [Python Rogue Tutorial](https://rogueliketutorials.com).

These tutorials are great because they build up the game mechanics iteratively. They also cover a broad range of topics beyond just game mechanics including UI, Loading/Saving, and System Design. After completing this tutorial you should have a simple, yet functional foundation to build on your own ideas.

## Tutorial Overview
1. We will use traditional sprites instead of ASCII characters for art. 
2. We will not use any external libraries and will rely solely on functionality in vanilla Gamemaker. This means we will build our own solutions to line of sight and pathfinding problems.
3. This will be an *intermediate* level tutorial, that a beginner should be able to follow along with. You should be familiar with GML and be comfortable navigating the IDE. We will attempt to build things in an extensible and introduce/demonstrate topics like design patterns and object-oriented programming principles.

## Conventions
There are lots of naming conventions and styles. For this project I will be using the following:

1. Objects will be `objNameOfThing`
2. Sprites will be `sprNameOfSprite`
3. Scripts will be `ScriptName`
4. Structs will be `NameOfStruct`
5. Methods will be `thisIsAMethod`
6. Member variables will be `thisIsMyVariable`
7. Local variables (`var`) will be `_localVariable`
8. Rooms will be `rmRoomName`

For common actions such as: Creating a new Object, Adding an Event to an Object, or Importing a Sprite - the tutorial will state the action simply and will not describe in detail how to navigate to or accomplish the task. If the task is more specialized, there will be more descriptions. 

**What will I learn if I complete this**
* Turn based games in Gamemaker
* The foundations of a traditional roguelike in Gamemaker
* Procedural generation
* Line of sight algorithms
* Different intermediate level code structure techniques


## Set up
1. Install the latest monthly version of Gamemaker (v2024.14.0.190)
2. Open GameMaker
3. Create a new blank project called "RogueTutorial"
4. Open the project up
5. Make sure you have an Objects, Rooms, Sprites, Script folders in the Asset Browser. You can have more but these will be used in the tutorial
6. Open the Rooms folder and rename the Room1 to rmDungeon
7. Make sure the room size is set to 1366x768

Tutorial Outline
1. Overview and Setup (You are here)
2. Draw character and move
3. Generic entity, rendering, map
4. Proc gen map
5. Field of View
6. Enemies
7. Damage / Combat
8. GUI
9. Items and Inventory
10. Scrolls and Targeting
11. Saving / Loading
12. Delving into the Dungeon
12. Increasing Difficulty
13. Gearing up
  
Extras
