---
layout: post
title: Collect Call mapping software
description: Make your own maps without writing any code!
image: assets/images/cc_tiled.png
---
While I was creating _Collect Call_, I ended up writing some code to interpret _Tiled_ levels to be playable in my game. Though the format needs to be very similar to my own levels, it is possible to load custom levels into the game.

You can download a kit full of mapping assets <a href="https://codeload.github.com/dbwrush/CollectCallMappingKit/zip/refs/heads/main" download>here,</a> or go to the <a href="https://github.com/dbwrush/CollectCallMappingKit">GitHub repository</a>.

If you're interested, here's all the details for making custom levels.

Currently, _Collect Call_ isn't available to the public, but it should be on Steam in about 3 months! If you'd like early access, message me on Discord or by email.
The below is copied from the ReadMe for the mapping kit.

# Collect Call Mapmaking Kit

This repository contains all the assets used to create levels for Collect Call. Before you start creating your own maps, it's recommended to explore some existing ones to understand the process.

You can download Tiled from https://mapeditor.org

## Opening Existing Maps

In the `maps` folder, you'll find 15 maps created for Collect Call. You can open these maps using Tiled. If you encounter red Xs, don't worry; that just means Tiled couldn't automatically find the Tilesets. All Tilesets for these maps are in the `tileset` folder. Locate the missing files by following the prompts in Tiled.

## Making Your Own Maps
When you distribute your map, you will need to include the map's .tmx file along with any other assets needed to run it. That includes any custom textures, dialogue files, etc. These should all be in one folder together.
Collect Call scans two places for map folders. The first is the Custom folder located in the same directory as the game's .jar file, and second is the content you've downloaded from Steam Workshop. Collect Call scans for .tmx files with names starting with "cc_".
The maps that are found are displayed in the game select screen by their file names with the "cc_" and ".tmx" removed and any underscores replaced with spaces. A map named "cc_Collect_Call.tmx" would be displayed as "Collect Call", so use this to set your mod's name! If your mod contains multiple maps, the later maps do not need to follow any specific naming convention. Maps are loaded when the game is prompted to open a map file by a LevelSwitch object.

## Map Properties

Each map has properties that control how the game interprets it. Important parameters include:

- `defaultZoom`: Controls the default camera zoom (usually a decimal between 0.1 and 0.3).
- `weatherParticleTextures`: Path to weather particle textures (e.g., "textures/raindrop.png").
- `weatherParticlesSpawnAngle`: Angle for particle spawn relative to the player.
- `weatherParticlesSpeed`: Speed of particle movement.
- `weatherParticlesSpread`: Spread of particles when spawned.

## Layers in Tiled

Tiled organizes maps into layers. Tile layers contain textures, and object layers store data about map locations.

### Tile Layers

Your maps can have multiple tile layers for a parallax effect. Tile layers don't require special properties.

### Object Layers

Object layers store data about map locations, including required and optional layers:

#### Required Object Layers:

- `EntitySpawner`: Controls entity spawn points.
- `LevelCollision`: Defines the world's collision.
- `LevelSwitch`: Manages level switching, including game-ending transitions.

#### Optional Layers:

- `Hints`: Provides button prompts.
- `CameraModes`: Overrides default camera behavior.
- `PuzzleObjects`: Places puzzles in the level.
- `Ladders`: Controls player ladder interactions.

For detailed information on each layer and its properties, see the sections below.

## EntitySpawner

Objects here control the placement and hitboxes of entities:

- `speed`: Speed of the entity.
- `textureHeight`: Height of the entity's texture.
- `texturePath`: Path to the entity's texture/animation.
- `textureWidth`: Width of the entity's texture.
- `type`: Sets entity class (Player, Follower, or Prop).

## LevelCollision

Objects represent the world's collision. No extra properties necessary.

## LevelSwitch

Controls switching to other levels, including game-ending transitions:

- `destinationMapFilePath`: Path to the next level. Enter "modpath/" for the folder your custom map is in. Example: "modpath/dialogues/example.json". 
- `destinationX`: Player's x-axis upon switching.
- `destinationY`: Player's y-axis upon switching.

## Hints

Provides button prompts:

- `button`: Button prompt (A, B, X, Y, Left, Right, Up, Down, StickUp, Start).
- `showDist`: Distance for button prompt visibility.

## CameraModes

Overrides default camera behavior:

- `cameramode`: "center" or "follow" for camera behavior.
- `targetzoom`: Camera zoom distance.

## DialogueObjects

Place interactable dialogues in the world. Dialogues are written as .json files. This kit contains an example one.

- `rootFilePath`: Path to the .json file containing this dialogue. Enter "modpath/" for the folder your custom map is in. Example: "modpath/dialogues/example.json". 
- `textureFilePath`: Path to the texture for this object. Set it to an empty string to make it invisible.


## PuzzleObjects

PuzzleObjects are used to place puzzles within the level. They include required and optional properties.

### Required Properties:

- **puzzlePieceID (INT):** Unique identifier for a puzzle piece, starting from 0. Do not skip IDs to avoid game crashes.
- **textureFilePath (STRING):** Path to the texture file. Must be present; set as an empty string for an invisible object.
- **type (STRING):** Controls the type of puzzle object loaded.
- **deactivateAfterSeconds (FLOAT):** Object remains active for this duration; not used by all puzzle types.

### Optional Properties:

- **animateOnInternalActivation (BOOLEAN):** Animation unaffected by the defaultActivated property.
- **defaultActivated (BOOLEAN):** Enables the object when it would be inactive and vice versa.
- **flipped (BOOLEAN):** Flips the object's texture.
- **or (BOOLEAN):** If enabled, only 1 item in the activators list is needed to activate this object, rather than all of them.
- **passiveAnimationFilePath (STRING):** Path to an animation the object should play when inactive.
- **permanentlyActivated (BOOLEAN):** If enabled, the object stays active forever.
- **loopAnimation (BOOLEAN):** If enabled, the object plays its animation as long as it is activated.

## PuzzleObject Types:

### PuzzleListener:

Generic puzzle object activated by other puzzle objects.

- **activators (STRING):** Comma-separated list of puzzle object IDs that this object should listen to.
  
  #### Subtypes:

  - **Clock:**
    - **onTime (FLOAT):** Length of time, in milliseconds, that it should be on.
    - **offTime (FLOAT):** Length of time, in milliseconds, that it should be off.

  - **FlagSetter:**
    - **flags (STRING):** Comma-separated list of flags this object should set.

  - **PuzzleCollision:**
    - #### Subtypes:
  
      - **MovingPlatform:**
        - **absoluteActivatedPos (BOOLEAN - optional):** Activated position relative to the map rather than the object's default position.
        - **activatedX (FLOAT):** Sets where the object will move on the X-axis when activated.
        - **activatedY (FLOAT):** Sets where the object will move on the Y-axis when activated.
        - **time (FLOAT):** Time, in seconds, for the object to move between positions.

      - **ToggleCollision:**
        - No special properties.

### EntityListener:

Generic puzzle object activated by entities.

- **activators (STRING):** Comma-separated list of entity types that this object should listen for.
- **gravity (FLOAT):** Strength that this object will pull activators towards it. Use a negative number to repel activators.

  #### Subtypes:

  - **FlagListener:**
    - **activators (STRING):** Comma-separated list of flag strings the object should listen for.

  - **UseListener:**
    - **highlightAnimation (STRING):** Path to an alternate texture/animation that should show when the player gets close.

## Ladders

Set similar to LevelCollision; player gets ladder controls when overlapping a rectangle object in the Ladders layer.
