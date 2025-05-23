---
title: Multiple Target Rendering to Outline Enemies Fast
date: 2025-05-22 16:00
categories: [Plush Rangers, Devlog]
tags: [gamedev, gamemaker, shaders]     # TAG names should always be lowercase
description: Outlining can help your player keep track of important enemies. This technique allows you to outline any object in a single pass.
---

[Plush Rangers](https://store.steampowered.com/app/3593330/Plush_Rangers/) outlines the enemies, characters, and some other artifacts to help them stand out a bit on a busy screen. It also helps with the perspective camera to allow the player to see enemies or items that are obscured by trees or other map elements.

![image.png](/assets/img/posts/20250523/outlineexample.png)

When I first approached drawing the outline, I ran into a couple of challenges:

1. If I did the outline when I drew the sprite, I could overwrite the outline with a later drawn sprite. This would ruin the “see-through” effect that the outline provides
2. If I did all outline drawing at the end, I had to redraw many elements a second time. Because of the number of entities in the game this would double the effort to draw each frame.

The solution I will share allows me to perform all the outlines with a single draw call and allows me to have outlines of a different color.

### Multiple Render Targets

The solution I landed on uses multiple render targets in the shader to have 2 surfaces that I draw to while drawing all my entities.

1. The standard application surface → This continued to render the sprite as normal without any outline considerations and does all the standard effects I wanted for drawing
2. The outline surface → I rendered a solid color version of the sprite in the outline color. 

During the post-draw event before presenting the application surface to the screen, I make a final pass rendering the outline surface onto the application surface with a standard outline shader. That shader than overlays the outlines onto the application surface, giving the outline effect to the entities.

### Code Snippets

Because there is a lot of extra work that happens around drawing the scene, I will just show the relevant snippets of code for outlining. These won’t work on their own, but hopefully will piece together the steps necessary to duplicate the effect.

***NOTE**: Before all this, I take over drawing the `application_surface` manually with `application_surface_draw_enable(false);`*

**1. In my camera that handles the rendering of the view, I set up the multiple target ([surface_set_target_ext](https://manual.gamemaker.io/monthly/en/#t=GameMaker_Language%2FGML_Reference%2FDrawing%2FSurfaces%2Fsurface_set_target_ext.htm))**

```jsx
// Camera Draw Begin Event
... 

// Make sure the surface is still good
surfaceOutline = surfaceValidate(surfaceOutline, window_get_width(), window_get_height());

// Clear it out
surface_set_target(surfaceOutline);
draw_clear_alpha(c_black, 0);
surface_reset_target();

// Set it as an additional target for rendering
surface_set_target_ext(1, surfaceOutline);

...
```

**2. While drawing my objects that are outlined I set up a shader that knows about this additional render target, and accepts an `outlineColor`value to use.**

```jsx
// Draw object (Simplified version of code)
// showOutline, outlineRed, outlineGreen, outlineBlue are all properties for the instance
shader_set(shdMultiTargetOutline);
shader_set_uniform_i(_uOutline, showOutline);
shader_set_uniform_f(_uOutlineColor, outlineRed, outlineGreen, outlineBlue, 1);

draw_self();

shader_reset();
        
```

```glsl
//
// Fragment Shader
//
varying vec2 v_vTexcoord;
varying vec4 v_vColour;

uniform int uDrawOutline;
uniform vec4 uOutlineColor;

void main()
{
		// Other shader code here.....
		// gl_FragData[0] is assigned the correct sprite color and 
		// alpha discard tests are performed
   ...
    
	  // Check if outline is enable, draw outline color with original sprite alpha value
	  // to allow smooth outlining
    if(uDrawOutline != 0) {
   	    gl_FragData[1] = vec4(uOutlineColor.rgb, gl_FragData[0].a);
	  }
}

```

**3. After all drawing is completed, the camera takes the outline surface and uses an outline shader to draw the outline surface onto the `application_surface`**

```jsx
// Camera Post Draw Event

// Draw the application surface because we are handling it manually
gpu_set_blendenable(false);
draw_surface(application_surface, 0, 0);
gpu_set_blendenable(true);

// Set our outline shader and draw the outline surface
shader_set(shOutlineSurface);
var _tex = surface_get_texture(surfaceOutline);
var _w = texture_get_texel_width(_tex);
var _h = texture_get_texel_height(_tex);

shader_set_uniform_f(uTexSize, _w, _h);
draw_surface_stretched(surfaceOutline, 0, 0, window_get_width(), window_get_height());

shader_reset();
```

This outline shader is from [DragoniteSpam’s Sobel filter video](https://youtu.be/7LxJk67rS8o?si=bsfVggbWHPR-uS_P): 

```glsl
varying vec2 v_vTexcoord;
varying vec4 v_vColour;

uniform vec2 uTexSize;

void main()
{
	vec3 color = vec3(0.0 , 0.0 , 0.0 );
	float mag = 1.; // This value can be manipulated to adjust the color of the outline
	
	mat3 sobelx = mat3(
		1.0,	2.0,	1.0, 
		0.0,	0.0,	0.0, 
		-1.0,	-2.0,	-1.0
	);
	mat3 sobely = mat3(
		1.0,	0.0,	-1.0, 
		2.0,	0.0,	-2.0, 
		1.0,	0.0,	-1.0
	);
	
	mat3 magnitudes;
	
	for (int i = 0; i < 3; i++) {
		for(int j = 0; j < 3; j++) {
			vec2 coords = vec2(v_vTexcoord.x + (float(i) - 1.0) * uTexSize.x, 
								v_vTexcoord.y + (float(j) - 1.0) * uTexSize.y);
			magnitudes[i][j] = length(texture2D(gm_BaseTexture, coords).a);
			color.rgb = max(color.rgb, texture2D(gm_BaseTexture, coords).rgb);
		}
	}
	
	float x = dot(sobelx[0], magnitudes[0]) + dot(sobelx[1], magnitudes[1]) + dot(sobelx[2], magnitudes[2]);
	float y = dot(sobely[0], magnitudes[0]) + dot(sobely[1], magnitudes[1]) + dot(sobely[2], magnitudes[2]);
	
	float final = sqrt(x * x + y * y) / 4.;
	
	gl_FragData[0] = vec4(color.rgb * mag, final);
}
```

### Final Result

The final result seems to perform well and allow me to outline any object I’d like. For example, all the experience point stars now can have outline associated with them for no extra effort.

![Lots going on, but those duckies cannot hide!](/assets/img/posts/20250523/OutlineActionScreenshot.png)

Let me know what you think!

### About Plush Rangers

![Plush Ranger Capsule](/assets/img/prCapsule1.png)

**Plush Rangers** is a fast-paced auto battler where you assemble a team of **Plushie Friends** to battle quirky mutated enemies and objects. Explore the diverse biomes of **Cosmic Park Swirlstone** and restore **Camp Cloudburst**!

Each plushie has unique abilities and behaviors—from the defensive **Brocco**, who repels nearby enemies, to **Tangelo**, a charging berserker tortoise. Level up your plushies to boost their power and unlock new capabilities.

Please consider supporting these Dev Logs by wish listing Plush Rangers on Steam: [https://store.steampowered.com/app/3593330/Plush_Rangers/](https://store.steampowered.com/app/3593330/Plush_Rangers/)
