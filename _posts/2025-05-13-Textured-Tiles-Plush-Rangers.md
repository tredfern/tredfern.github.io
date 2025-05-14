---
title: How to use a Shader to Add Textures to your Tiles in Gamemaker 
date: 2025-05-13 16:00
categories: [Plush Rangers, Devlog]
tags: [gamedev, gamemaker, shaders]     # TAG names should always be lowercase
description: This is a simple technique that can add a lot of extra detail to your game with not much effort.
---
**Sample Project Here!**
[Github](https://github.com/tredfern/BlendedTerrain) | [YYZ](https://github.com/tredfern/BlendedTerrain/releases/download/20250513/BlendedTerrain.yyz)

### Overview
For Plush Rangers, we wanted our ground to have a hand-drawn feel while allowing quick level and map creation—with the potential for procedural generation later.

GameMaker excels at tile handling, so we explored that approach first. However, when we added tile variations, the result looked too gridded. Here's an early prototype of the ground.

![Tiles with grid appearance](/assets/img/posts/20250514/griddedtiles.png)

While we could have refined the tiles with smoother edges, that approach wasn't achieving the natural look we wanted. Our goal was to create something artistic yet clean. This is where we have ended up.

![layering.png](/assets/img/posts/20250514/layering.png)

We are still using tiles for the paths and the shape of the areas, but if you look at the grass you would have a hard time finding the line where the grass repeats. Along the edges of the grass there is some extra texturing from the tiles to help the transition between different tilesets.

**Here is how you can achieve a similar effect in your own games!**

## Setup

To draw textured tiles you need three things

1. A grayscale tileset (it can have other colors, but grayscale works most intuitively)
2. A texture
3. A shader to blend the terrain together

## Images

Here are examples of the tiles and texture.

The tileset can be any format or size you want. It depends on the look you want to achieve in your game. These are 64x64 tiles set up for 16-autotile.

![Grayscaletiles.png](/assets/img/posts/20250514/Grayscaletiles.png)

Your texture can be any square shape. We use 1024x1024 textures. Because of the way tiles and texture interacting that means our texture will repeat every 16 tiles.

![Grass Texture](/assets/img/posts/20250514/grass_texture_reduced.png)

## Gamemaker

I set up [my tiles as normal](https://gamemaker.io/en/tutorials/tile-set-editor) in Gamemaker and draw them into the editor. 

![Editor image](/assets/img/posts/20250514/editor2.png)

### Tile Map Create Code

When the room is loaded I get the tilemap layers and set them to hidden. I want to handle drawing them myself. In the sample, I only have a single layer, but these could keep layering on top of each other.

```jsx
/// CREATE EVENT (Plus a script file for the function)
function layerGetTilemaps() {
	var _layers = layer_get_all();
	var _tmLayers = [];
	
	for(var _i = 0; _i < array_length(_layers); _i++) {
			// If layer is marked hidden in the editor, we should ignore it
			if(layer_get_visible(_layers[_i])) {
				if(layer_tilemap_get_id(_layers[_i]) != -1) {
					array_push(_tmLayers, _layers[_i]);	
				}
			}
	}
	
    // We get the array front to back, we want to draw back to front
	return array_reverse(_tmLayers);
}

tilemapLayers = layerGetTilemaps();
// Hide tilemap layers, we'll take it from here
array_foreach(tilemapLayers, function(_l) { layer_set_visible(_l, false); });
```

### **Tilemap Drawing Code**

When drawing the tiles, loop through each tileset, check if it has a blend texture associated with it. If it does, I set up the shader to blend it and then draw the tileset. The most important part in this routine besides passing the texture in, is making sure to pass the proper coordinates for the texture. 

```jsx
/// DRAW EVENT
// Get parameters for our shader
var _sampler = shader_get_sampler_index(shdBlendTerrain, "uTerrainTexture");
var _texCoord = shader_get_uniform(shdBlendTerrain, "uTerrainTexcoord");
var _texSize = shader_get_uniform(shdBlendTerrain, "uTexSize");
var _uBlendRate = shader_get_uniform(shdBlendTerrain, "uBlendRate");

// Tile Map Drawing
for(var _tileLayer = 0; _tileLayer < array_length(tilemapLayers); _tileLayer++) {
	var _tileId = layer_tilemap_get_id(tilemapLayers[_tileLayer]);
	
	// Figure out what texture sprite to use for blending or undefined to bypass blending
	var _blendTexture = getBlendTexture(_tileId);
	
	if(_blendTexture != undefined) {
		shader_set(shdBlendTerrain);
		
		// Pass in the texture to the shader
		texture_set_stage(_sampler, sprite_get_texture(_blendTexture, 0));

		// Need to get the specific texture coordinates for the texture from the page
		var _uvs = sprite_get_uvs(_blendTexture, 0);
		shader_set_uniform_f(_texCoord, _uvs[0], _uvs[1], _uvs[2], _uvs[3]);

	  // Assumes a square texture
		shader_set_uniform_f(_texSize, sprite_get_width(_blendTexture)); 

		// Blending between tilelayer and texture, 1 for most cases
		shader_set_uniform_f(_uBlendRate, 1); 
	}
	
	draw_tilemap(_tileId, 0, 0);
	shader_reset();
}
```

### Shader Code

The vertex shader does one important thing. It sets a `varying` value for the position. Varying values will interpolate allowing us to figure out what world position makes most sense for our texture coordinates.

```glsl
//
// Terrain Blending Vertex Shader
//
attribute vec3 in_Position;                  // (x,y,z)
//attribute vec3 in_Normal;                  // (x,y,z)     unused in this shader.
attribute vec4 in_Colour;                    // (r,g,b,a)
attribute vec2 in_TextureCoord;              // (u,v)

varying vec2 v_vTexcoord;
varying vec4 v_vColour;
// The key addition to the vertex shader
varying vec3 v_vPosition;

void main()
{
    vec4 object_space_pos = vec4( in_Position.x, in_Position.y, in_Position.z, 1.0);
    gl_Position = gm_Matrices[MATRIX_WORLD_VIEW_PROJECTION] * object_space_pos;
    
    v_vColour = in_Colour;
    v_vTexcoord = in_TextureCoord;
    // Set the varying position for the fragment shader
	  v_vPosition = in_Position;
}

```

The fragment shader uses a simple algorithm to figure out the proper color to use:

1. *Get the texture coordinates*  
    This is based on the world position assuming a 1 to 1 relationship between sprite pixel size and world coordinates. For example, with a 1024x1024 texture, and a tile at 1040, 500 -> we need the texture coordinate for 16, 500.

    The texture coordinates are then normalized (0..1) and adjusted for the texture page. (You can simplify this step by setting your textures to live on their own texture page, but I try to let GM handle the image data as much as it can)
2. We get the color based from the tileset (`baseColor`)
3. We get the color from the the texture (`textureColor`)
4. We combine them together to get our final color. This allows the tiles to have some edge to them that we can see or adjust. We could have different shapes, or if we had water we might have animated tiles that change that would allow more variation. We also use the `alpha` value from the base color to figure out what areas should not be drawn.

```glsl

//
// Terrain Blending Fragment Shader
//
varying vec2 v_vTexcoord;
varying vec4 v_vColour;
varying vec3 v_vPosition;

uniform sampler2D uTerrainTexture;
uniform vec4 uTerrainTexcoord;
uniform float uTexSize;
uniform float uBlendRate;

void main()
{
  // Intensity is usually == vec4(1.). 
	vec4 intensity = vec4(uBlendRate, uBlendRate, uBlendRate, 1.);
	
	// Figure out the correct texture coordinates in our texture
	// This calculates a texture coordinate based on world position
	// eg. If our texture is 1024x1024. And we are at world position 2052, 100
	//       We are looking around for the pixel at 4/1024, 100/1024
	vec2 terrainCoord = mod(v_vPosition.xy, uTexSize) / uTexSize;
	
	// Get the specific texture coordinate from the texture page for the terrain
	vec2 c = uTerrainTexcoord.xy + (uTerrainTexcoord.zw - uTerrainTexcoord.xy) * terrainCoord;
	
	// Base color is the color from the tile. Usually a grayscale value. 
	// The base color also defines how much alpha channel will be used so transparent areas
	// of the tile are not drawn
	vec4 baseColor = texture2D( gm_BaseTexture, v_vTexcoord );

	// Get the texture color from the coordinates we calculated
	vec4 textureColor = texture2D( uTerrainTexture, c );
	
	// Figure out the combination of all those colors together
	vec4 blendColor = baseColor * textureColor * intensity; 
	
	// Set the color, blending with anything provided from the vertex (hopefully white)
	gl_FragData[0] = v_vColour * vec4(blendColor.rgb, baseColor.a);

}
```

## The Results 

If you download and run the sample code you should see this:

![image.png](/assets/img/posts/20250514/blendedterrainexample.png)


I think this is a great and simple technique for giving tiles an organic feel without the burden of tons of tiles. You can use similar tiles and texture them to give different appearances, such as different kinds of wooden floors. There are a lot of applications for this technique depending on the kind of game you are making. 

### About Plush

![Plush Ranger Capsule](/assets/img/prCapsule1.png)

**Plush Rangers** is a fast-paced auto battler where you assemble a team of **Plushie Friends** to battle quirky mutated enemies and objects. Explore the diverse biomes of **Cosmic Park Swirlstone** and restore **Camp Cloudburst**!

Each plushie has unique abilities and behaviors—from the defensive **Brocco**, who repels nearby enemies, to **Tangelo**, a charging berserker tortoise. Level up your plushies to boost their power and unlock new capabilities.

Wishlist Plush Rangers on Steam: https://store.steampowered.com/app/3593330/Plush_Rangers/
