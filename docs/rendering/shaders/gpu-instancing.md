---
title: "GPU Instancing"
icon: "⚡"
created: 2024-12-05
updated: 2026-07-29
---

# GPU Instancing

GPU instancing is an optimization where multiple instances of the same model with the same material can be drawn in a single draw call. This is a big optimization when rendering things that appear many times in a scene such as foliage.

## Standard Instancing

Everything is automatically instanced by default, if the renderer can batch your models into 1 draw call, it will.

### Instance ID

Standard vertex input comes with a property for ID of your instance already, it is called `i.nInstanceTransformID`. Bone index (1D blend index for skinned meshes) can be fetched from `i.nBoneIndex`.

To add batched instance ID and bone index to your custom vertex struct, add this:
```cpp
uint nInstanceTransformID : TEXCOORD13 < Semantic( InstanceTransformUv ); >;
uint nBoneIndex 		  : BLENDINDICES < Semantic( BlendIndices ); >;
```

### Helpers

* To get the Object→World matrix in your vertex shader you use the following function:

```cpp
float3x4 GetTransformMatrix( uint nTransformIndexID, uint nBlendIndex = 0 )
```

* Extra per instance data can be grabbed by using:

```cpp
ExtraShaderData_t GetExtraPerInstanceShaderData( uint nTransformIndexID );
```

ExtraShaderData allows you to still have instances even though some data is different between them, currently only tint color is changeable within instances:

```cpp
struct ExtraShaderData_t
{
	float4 vTint;
	uint nBlendWeightCount;	// if D_SKINNING, blend weight count
};
```

* There's also a helper function which allows quickly getting transform scale from object->world matrix of your instance:
```cpp
float3 ComputeMatrixScale( float3x4 m )
```

## Procedural Instancing

Procedural instancing is useful for programmatic shaders that are to be invoked from C# or indirectly.
No transforms are passed, only the count of instance ids, you would derive some form of transform yourself from them within the shader.

You can get the instance id from SV_InstanceID in your vertex function. This is also useful for indirect draw calls.

```cpp
PixelInput MainVs( VertexInput i, uint instanceID : SV_InstanceID )
{ 
    float3 offsetPosition = float3( 0, 0, 64 * instanceID );
}
```

See:

* `Graphics.DrawModelInstanced( Model, count )`
* `Graphics.DrawModelInstancedIndirect( Model, GpuBuffer )`
