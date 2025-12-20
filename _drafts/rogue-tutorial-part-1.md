# Part 1 - Drawing a player on the screen, at the correct scale, and move it around

Welcome! Let's get started.

This first chapter, we will create a player that will move around in a grid based movement. It won't be much, but this will set up some of the foundations for the code we will be using for the rest of the tutorial.

We are going to be using 16x16 tiles for this tutorial. This gives enough pixels to make characters and objects, but not too many to bog us down when we need to make graphics. It should have a nice retro-pixel graphic look to it.

## The player

Let's start by getting the player created and drawing to the screen


1. Download the Sprite asset (or create your own 16x16 character sprite)
2. Drag the sprite file into GameMaker and drop on the "Sprites" folder
3. Rename the sprite `sprPlayerIdle`
4. Create a new Object name `objPlayer` and assign it's sprite 
5. Inside the Asset Browser, right click on the Objects folder and select Create->Object
6. Name this object `objPlayer`
7. Open `rmDungeon` and drag the objPlayer object into the Room somewhere

Try running the game. You should see an itsy-bitsy image on the middle of the screen. That's not going to be much fun. Let's reconfigure the viewports before we go any farther.

To solve this let's create a camera object that will scale the graphics to the correct size for us.

## The Camera

1. Create a new object and call it `objCamera`
2. Add a create event and put in the following code

```
/// @description Create the camera view

// Set the variables for our object
viewWidth = window_get_width();
viewHeight = window_get_height();
cameraWidth = 640;
cameraHeight = 360;
trackObject = objPlayer;

// Define the view port
view_enabled = true;
view_set_visible(0, true);
view_set_xport(0, 0);
view_set_yport(0, 0);
view_set_wport(0, viewWidth);
view_set_hport(0, viewHeight);

// Create the camera
camera = camera_create_view(x, y, cameraWidth, cameraHeight);
view_set_camera(0, camera);
```

3. Open up your room and add the `objCamera` object to the room. 

If you run it now, the player should look more visible on the screen
