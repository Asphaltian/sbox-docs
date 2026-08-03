---
title: "Achievements"
icon: "🏅"
created: 2024-09-10
updated: 2026-08-03
---

# Achievements

Your game can have multiple achievements for players to unlock.

Achievements are not defined in code. You create them on your game package's page on sbox.game, under `Services > Achievements`, and your game code only refers to them by their ident. Each achievement has an ident, a title, a description, an icon, a score, and an unlock mode of either **Manual** or **Stat**.

The API lives in the `Sandbox.Services` namespace.

## Score

Each achievement can have a different score. The score is usually between 5 and 100, which is really something you need to choose based on the achievement. Generally, give low values to easy-to-achieve things and a high value to hard-to-achieve things. Have many low value, and few high value.

When a player unlocks the achievement, the score is added to their global score.

You have full control over choosing the score but the total combined for your game cannot exceed 1000.


## Icons

The icon is automatically resized to 128x128. It will generally be rendered quite small, next to the name of the achievement, so should be treated more like an icon than a picture.


## Stat Based

When your achievement unlock mode is set to "Stat" it will automatically be unlocked for you. All you need to do is make sure the stat is set up properly.

An example of a stat based achievement would be the "100 coins".

You would do `Stats.Increment( "coins", 1 )` in your code every time you collected a coin, then you can set your achievement to this..

| Property | Value | Explanation |
|----------|-------|-------------|
| Target Stat | "coins" |             |
| Aggregation | Sum   |             |
| Min Value | 0     |             |
| Max Value | 100   |             |
| Show Progress | true  |             |

* Target Stat: "coins" (the name of the stat)
* Aggregation: Sum (you want to add the 1's values together)
* Min Value: 0, Max Value: 100 (unlocks at 100)
* Show Progress - yes - will show progress between 0 and 100 as a percentage

Stat achievements are polled in the background, so an unlock happens shortly after the stat crosses the threshold rather than on the exact frame. You don't need to do anything else.


## Manual

If the achievement is set to manual, then you can unlock it in your code like this:

```csharp
Sandbox.Services.Achievements.Unlock( "achievement_ident" );
```

Unlocking is per local player, so this call does nothing on a dedicated server. Call it on the client that earned the achievement.


# Listing

You can get the list of achievements at any time in your game. This is what you'd use to build your own achievement UI.

```csharp
foreach ( var a in Sandbox.Services.Achievements.All )
{
    Log.Info( $"{a.Title}: {a.Description} ({a.Score} points)" );

    if ( a.IsUnlocked )
        Log.Info( $"  unlocked {a.UnlockTimestamp}" );
    else if ( a.HasProgression )
        Log.Info( $"  {a.CurrentValue} / {a.Range.y} ({a.ProgressionFraction:P0})" );
}
```

Each `Achievement` has the following properties:

| Property | Description |
|----------|-------------|
| `Name` | The achievement's ident, the same string you pass to `Unlock` |
| `Title` | Display name |
| `Description` | Display description |
| `Icon` | URL of the icon |
| `Score` | How many points this is worth |
| `IsUnlocked` | Whether the local player has unlocked it |
| `UnlockTimestamp` | When it was unlocked, `null` if it hasn't been |
| `IsVisible` | False for hidden achievements that haven't been unlocked yet |
| `HasProgression` | Whether this achievement tracks progress (stat based with Show Progress on) |
| `Range` | The Min Value (`x`) and Max Value (`y`) configured for a stat achievement |
| `CurrentValue` | The player's current value towards `Range` |
| `ProgressionFraction` | Progress from `0` to `1` |
| `GlobalUnlocked` | How many players have unlocked it |
| `GlobalFraction` | Fraction of players who have unlocked it, useful for showing rarity |

## Map Achievements

Maps are packages too, so a map can define its own achievements separately from the game it runs on. Everything above applies, just under `Achievements.Map`:

```csharp
Sandbox.Services.Achievements.Map.Unlock( "found_the_secret" );

var achievement = Sandbox.Services.Achievements.Map.Get( "found_the_secret" );

foreach ( var a in Sandbox.Services.Achievements.Map.All )
{
    // ...
}
```

These resolve against whichever map package is currently loaded, so a game that loads community maps will see a different set per map.

## Web API

Or from outside the game, you can access it via the API

```none
https://public.facepunch.com/sbox/achievement/list?package=facepunch.testbed
```
