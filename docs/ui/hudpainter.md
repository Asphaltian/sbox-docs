---
title: "HudPainter"
icon: "🎨"
created: 2024-11-07
updated: 2026-09-16
---

# HudPainter

> `HudPainter` is obsolete. It has been replaced by [Painter](painter/index.md), which does everything it did and much more.

`Camera.Hud` and `Camera.Overlay` used to give you a `HudPainter` with `DrawRect`, `DrawLine`, `DrawText` and `DrawTexture` methods. They still work for now, but you should switch to `BeginHud()` and `BeginOverlay()`, which give you a `Painter`.

The main difference is that fill, stroke and text style are state you set on the painter, rather than arguments you pass to each call. The painter must also be disposed to submit its drawing, so use `using`.

## Before

```csharp
protected override void OnUpdate()
{
	var hud = Scene.Camera.Hud;

	hud.DrawRect( new Rect( 300, 300, 10, 10 ), Color.White );
	hud.DrawLine( new Vector2( 100, 100 ), new Vector2( 200, 200 ), 10, Color.White );
	hud.DrawText( new TextRendering.Scope( "Hello!", Color.Red, 32 ), new Vector2( Screen.Width * 0.5f, 100 ) );
}
```

## Now

```csharp
protected override void OnUpdate()
{
	using var painter = Scene.Camera.BeginHud();

	painter.Fill = Color.White;
	painter.Rect( new Rect( 300, 300, 10, 10 ) );

	painter.Stroke = Stroke.Solid( Color.White, 10 );
	painter.Line( new Vector2( 100, 100 ), new Vector2( 200, 200 ) );

	painter.TextStyle = new TextStyle { FontSize = 32, Color = Color.Red, Alignment = TextFlag.CenterTop };
	painter.Text( "Hello!", new Rect( 0, 100, painter.Bounds.Width, 40 ) );
}
```

| Before | Now |
|--------|-----|
| `Camera.Hud` | `Camera.BeginHud()` |
| `Camera.Overlay` | `Camera.BeginOverlay()` |
| `DrawRect( rect, color )` | `Fill = color; Rect( rect );` |
| `DrawLine( a, b, width, color )` | `Stroke = Stroke.Solid( color, width ); Line( a, b );` |
| `DrawText( scope, position )` | `TextStyle = ...; Text( text, rect );` |
| `DrawTexture( texture, rect )` | `Texture( texture, rect );` |
| `DrawCircle( position, size, color )` | `Fill = color; Circle( position, size );` |
| `SetMatrix( matrix )` | `Transform = matrix;` or `Translate`, `Rotate`, `Scale` |
| `SetBlendMode( mode )` | `BlendMode = mode;` |

See [Destinations](painter/destinations.md) for the full guide to drawing on cameras.
