---
title: "GameObject"
icon: "📦"
created: 2023-11-14
updated: 2026-08-03
---

# GameObject

A `GameObject` represents an object in the scene world. It contains a few different elements.


# Name and Id

Every GameObject has a `Name`, which is what you see in the hierarchy, and an `Id` (a `Guid`) that is unique within the scene. The name is for you, the id is what the scene file and the network system use to refer to the object. See [Directory](/scene/scenes/index.md#directory) for looking objects up by id.


# Enabled and Active

`Enabled` is the checkbox you tick in the inspector. `Active` is whether the object is *actually* running: enabled, in a scene, and with every ancestor enabled too.

```csharp
GameObject.Enabled = false; // disables this object and everything under it

if ( GameObject.Active )
{
	// we and all our parents are enabled
}
```

Disabling a parent makes all of its children inactive without changing their own `Enabled` value, so re-enabling the parent restores them as they were.


# Transform

Represents where the GameObject is in the scene. Its positioning, its rotation and its scale.

If it has a parent then its transform is held relative to them, so when their parent moves, so does the child.

Here's how you can interact with them in code

```csharp
// Set world position
GameObject.WorldPosition = new Vector3( 100, 100, 100 );

// Set position relative to parent
GameObject.LocalPosition = new Vector3( 100, 100, 100 );

// Set world transform
GameObject.WorldTransform = new Transform( Vector3.Zero, new Angles( 90, 90, 180 ), 2.0f )
```


# Tags

The GameObject's tags are used for multiple things. They're used to group physics objects to decide what should collide with each other. They can be used by cameras to decide which objects should and shouldn't render. And they can be used by programmers to do whatever they want.

```csharp
if ( GameObject.Tags.Has( "enemy" ) )
{
	GameObject.Destroy();
}

GameObject.Tags.Add( "enemy" );
GameObject.Tags.Set( "enemy", isEnemy );
GameObject.Tags.Remove( "enemy" );
```

Tags are inherited. If a parent has the tag, then so does the child. The only way to remove the tag from the child is to remove it from the parent.


# Hierarchy

GameObject children are available via `GameObject.Children`. This is just a list of GameObjects.

To reparent an object, use `SetParent`. It keeps the object's world position by default, so it doesn't visually jump when you move it in the hierarchy.

```csharp
// Keeps its world position
child.SetParent( newParent );

// Keeps its local position instead, so it snaps to be relative to the new parent
child.SetParent( newParent, false );
```

There's also `Parent`, `Root`, `IsRoot`, and `IsAncestor` / `IsDescendant` for walking and testing the hierarchy.


# Creating and Cloning

```csharp
// A new empty object in the current scene
var go = new GameObject();

// Copy an existing object, optionally somewhere else
var copy = go.Clone();
var copyAt = go.Clone( new Vector3( 0, 0, 100 ) );
var copyFull = go.Clone( WorldPosition, WorldRotation );
```

This is also how you spawn [Prefabs](/scene/prefabs/index.md) at runtime.


# Destroying

`Destroy()` marks the object for deletion, and it's actually removed at the start of the next frame. This means it still exists for the rest of the current frame, so use [IsValid](/code/code-basics/is-valid.md) rather than a null check on anything that might have been destroyed.

```csharp
GameObject.Destroy();

// True as soon as Destroy() has been called, before it's actually gone
if ( GameObject.IsDestroyed ) return;
```

There's also `DestroyImmediate()`, which removes it right now. Avoid it unless you know nothing else is mid-way through using the object.


# Components

GameObjects implement functionality using [Components](/scene/components/index.md).


# Networking

`NetworkMode` decides whether and how the object is sent to other players, and `Network` is where ownership and refreshing live. See [Networked Objects](/networking/networked-objects.md) and [Ownership](/networking/ownership.md).
