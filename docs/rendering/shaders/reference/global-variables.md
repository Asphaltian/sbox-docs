---
title: "Global Variables"
icon: "🔢"
created: 2023-11-26
updated: 2026-07-30
---

# Global Variables

Shaders have access to a number of variables that are available from everywhere. They are defined per-view, meaning that some data will vary depending on current viewport. Compute shader can access this data as well, as long as they are being dispatched from render context.

:::info
I don't love these names but they're not going to go anywhere, so you're free to use them. In all likelihood in the future we'll get together and create a bunch of nicer #define's for them, so `g_flTime` will be accessible via`Time.Now`, or just `time` etc.
:::

:::info
This page only includes variables that are actually used by the engine and have useful purpose for shaders. You can look up a full list of all global per-view variables, including unused ones, here: [game/core/shaders/common.fxc](https://github.com/Facepunch/sbox-public/blob/master/game/core/shaders/common.fxc)
:::

# Time

| Type | Name | Description |
|------|------|-------------|
| `float` | `g_flTime` | Current time |

# Projections

These projection matrices are used for conversions between various coordinate spaces.

| Type | Name | Description | Helper Functions |
|------|------|-------------|------------------|
| `float4x4` | `g_matWorldToProjection` | Converts world-space coordinates to projection-space. In vertex shaders, it is common to convert world-space vertex position back to projection space before finalizing vertex manipulations | `Position4WsToPs`, `Position3WsToPs` |
| `float4x4` | `g_matProjectionToWorld` | Converts projection-space coordinates to world-space. Used in `Depth::GetWorldPosition` to reconstruct world-space position of ray from depth buffer | ... |
| `float4x4` | `g_matWorldToView` | Converts world-space coordinates to view-space. This coordinate system is built relative to the camera, with it as the origin | `Position4WsToVs`, `Position3WsToVs` |
| `float4x4` | `g_matViewToProjection` | Converts view-space coordinates to projection-space | `Position4VsToPs`, `Position3VsToPs` |
| `float4x4` | `g_matProjectionToView` | Converts projection-space coordinates to view space | ... |
| `float4x4` | `g_matCurrFrameViewToPrevFrameProj` | Converts view-space coordinates of current frame to projection-space coordinates of previous frame | `ReprojectFromLastFrameSs` |
| `float4` | `g_vInvProjRow3` | Row 3 of the inverse projection matrix | ... | 

# Camera

| Type | Name | Description |
|------|------|-------------|
| `float3` | `g_vCameraPositionWs` | Camera position in world space |
| `float3` | `g_vCameraDirWs` | Camera direction in world space |
| `float3` | `g_vCameraUpDirWs` | Camera up direction in world space |
| `float4` | `g_vCameraAngles` | Camera raw pitch/yaw/roll in radians, unused |
| `float`  | `g_flCameraFOV`| Camera FOV in radians |
| `float`  | `g_flNearPlane` | Camera's near plane |
| `float`  | `g_flFarPlane`  | Camera's far plane |

# Viewport 

| Type | Name | Description |
|------|------|-------------|
| `float` | `g_flViewportMinZ` | Depth buffer min range for current viewport |
| `float` | `g_flViewportMaxZ` | Depth buffer max range for current viewport |
| `float2` | `g_vViewportSize` | Viewport width (X) and height (Y) |
| `float2` | `g_vViewportOffset` | Viewport top left XY coords for offset if it's on a shared render target |
| `float2` | `g_vRenderTargetSize` | Render target width (X) and height (Y) |
| `float2` | `g_vInvViewportSize` | Reciprocal of original viewport size (`1.0 / g_vViewportSize`) |
| `float4` | `g_vFrameBufferCopyInvSizeAndUvScale` | Reciprocal of frame buffer size. X and Y are `1.0f / viewport size`, Z and W are always `1.0`. This property is considered unused, but you still may see it in some older shaders |  

# Other

| Type | Name | Description |
|------|------|-------------|
| `float4` | `g_vRandomFloats` | Random floats in each component, values change every frame |
