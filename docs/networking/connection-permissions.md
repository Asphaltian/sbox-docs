---
title: "Connection Permissions"
icon: "🔑"
created: 2024-03-21
updated: 2026-08-03
---

# Connection Permissions

The host can change some permissions for a specific `Connection`. The ideal place to set these permissions would be in the `OnActive` [network event.](/networking/network-events.md)

Only the host can set these. They're all `true` by default, and the host always has them regardless of what you set.

| Permission | Controls |
|------------|----------|
| `CanSpawnObjects` | Whether this connection can create its own networked objects |
| `CanRefreshObjects` | Whether this connection can send refresh updates for objects it owns |
| `CanDestroyObjects` | Whether this connection can destroy networked objects it owns |

# Spawning Objects

You can set `Connection.CanSpawnObjects` to allow or disallow a specific connection to create their own networked objects. By default this is `true`.


# Refreshing Objects

By default only the host can send network refresh updates for networked objects. This can be changed to allow the owner of a networked object to also send these updates with `Connection.CanRefreshObjects`.


# Destroying Objects

`Connection.CanDestroyObjects` controls whether a connection is allowed to destroy the networked objects it owns. Turn it off if the host should be the only one deciding when something is removed.

```csharp
public void OnActive( Connection channel )
{
	// Let clients own and move things, but not create or delete them
	channel.CanSpawnObjects = false;
	channel.CanDestroyObjects = false;
}
```


# Kicking

Permissions decide what a connection may do once it's in. To remove one entirely, the host can kick it:

```csharp
channel.Kick( "No cheating" );
```

To turn a connection away before it ever joins, use `AcceptConnection` instead - see [Network Events](/networking/network-events.md#acceptconnection).
