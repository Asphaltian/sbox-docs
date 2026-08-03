---
title: "Fog"
icon: "🌁"
created: 2024-12-08
updated: 2026-08-03
---

# Fog

This class applies atmospheric effects to a fragment.

Only useful if you're doing your own [Shading Model](/rendering/shaders/shading-model.md), otherwise this is already all implied when you use `ShadingModelStandard`.

# API

## Apply All 

```cpp
Fog::Apply( float3 WorldPosition, float2 ScreenPosition, float3 Color );
```

This will apply *all* types of atmospheric effects onto current pixel (`float3 Color`) if they are available on scene:
* Gradient fog
* Cubemap fog
* Volumetric fog 

If you only need specific fog effects, see the functions below. 

## Gradient Fog

```cpp
float3 ApplyGradientFog( float3 Color, float3 WorldPosition, float3 PositionToCamera );
```

For both **gradient** and **cubemap** fog, you need to calculate `PositionToCamera`. This can be done by simply subtracting world-space camera position from pixel world position:

```cpp
float3 PositionToCamera = worldPos.xyz - g_vCameraPositionWs;
```

## Cubemap Fog

```cpp
float3 ApplyCubemapFog( float3 Color, float3 WorldPosition, float3 PositionToCamera );
```

## Volumetric Fog

```cpp
float3 ApplyVolumetricFog( float3 Color, float3 WorldPosition, float2 ScreenPosition );
```

Each of these returns the fogged colour, they don't modify their input.
