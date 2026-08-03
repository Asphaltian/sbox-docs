---
title: "Post Processing"
icon: "🖼️"
created: 2024-05-08
updated: 2026-08-03
---

# Post Processing

Post-processing effects can be applied by adding components to the Camera or to a [PostProcessVolume](/rendering/post-processing/postprocessvolume.md).

## [Effects](/rendering/post-processing/effects/index.md)

The built in effects: tonemapping and film grain.

## [PostProcessVolume](/rendering/post-processing/postprocessvolume.md)

Apply a set of effects only while the camera is inside a volume, with blending.

## [Creating PostProcesses](/rendering/post-processing/creating-postprocesses.md)

Write your own effect as a component plus a shader.

# Camera Settings

The Camera component has settings especially for post processing.

### EnablePostProcessing

Disable to prevent post processing from rendering on this camera at all.

### PostProcessAnchor

By default when triggering `PostProcessVolume`'s we use the camera's position. 

This isn't always the behaviour you want. For example, if you're making a top down game, you probably want it to use the player's position.

If this is set, we'll use the world position of that GameObject to find PostProcessVolume's instead of the Camera's position.
