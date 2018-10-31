# Overlay

> danger
> The Discord Store is still in a beta period. All documentation and functionality can and will change.

Discord comes with an awesome built-in overlay, and you may want to make use of it for your game. This manager will help you do just that! It:

- Gives you the current state of the overlay for the user
  - Locked, enabled, unlocked, open, closed, etc.
- Allows you to change that state

## Data Models

###### ActivityActionType Enum

| value    |
| -------- |
| Join     |
| Spectate |

## IsEnabled

Check whether the user has the overlay enabled or disabled. You probably want to make this check before attempting to interact with the overlay.

Returns a `bool`.

###### Parameters

None

###### Example

```cs
if (!overlaymanager.IsEnabled())
{
  Console.WriteLine("Could not complete operation. Please enable the overlay and relaunch the game.");
}

else {
  DoTheThing();
}
```

## IsLocked

Check if the overlay is currently locked or unlocked

###### Parameters

None

###### Example

```cs
if (overlayManager.IsLocked())
{
  overlayManager.SetLocked(false);
}
```

## SetLocked

Locks or unlocks the overlay.

Returns `void`.

###### Parameters

| name   | type | description                |
| ------ | ---- | -------------------------- |
| locked | bool | lock or unlock the overlay |

###### Example

```cs
if (overlayManager.IsLocked())
{
  overlayManager.SetLocked(false);
}
```

## OpenActivityInvite

Opens the overlay modal for sending game invitations to users, channels, and servers. In order for this to function, you must have the proper activity fields set on the local client before attempting to use this feature. To know what you need for Spectate and Join invites, refer to [Activity Action Field Requirements](#DOCS_GAME_SDK_ACTIVITIES/activity-action-field-requirements).

Returns a `Discord.Result` via callback.

###### Parameters

| name | type               | description                 |
| ---- | ------------------ | --------------------------- |
| type | ActivityActionType | what type of invite to send |

###### Example

```cs
overlayManager.OpenActivityInvite(Discord.ActivityActionType.Join, (result) =>
{
  if (result == Discord.Result.OK)
  {
    Console.WriteLine("User is now inviting others to play!");
  }
});
```

## OpenGuildInvite

Opens the overlay modal for joining a Discord guild, given its invite code. An invite code for a server may look something like `fortnite` for a verified server—the full invite being `discord.gg/fortnite`—or something like `rjEeUJq` for a non-verified server, the full invite being `discord.gg/rjEeUJq`.

Returns a `Discord.Result` via callback. Note that a successful `Discord.Result` response does not necessarily mean that the user has joined the guild. If you want more granular control over and knowledge about users joining your guild, you may want to look into implementing the [`guilds.join` OAuth2 scope in an authorization code grant](#DOCS_TOPICS_OAUTH2/authorization-code-grant) in conjunction with the [Add Guild Members](#DOCS_RESOURCES_GUILD/add-guild-member) endpoint.

###### Parameters

| name | type   | description                |
| ---- | ------ | -------------------------- |
| code | string | an invite code for a guild |

###### Example

```cs
overlayManager.OpenGuildInvite("rjEeUJq", (result) =>
{
  if (result == Discord.Result.Ok)
  {
    Console.WriteLine("Invite was valid and overlay is open");
  }
});
```

## OnToggle

Fires when the overlay is locked or unlocked (a.k.a. opened or closed)

###### Parameters

| name   | type | description                           |
| ------ | ---- | ------------------------------------- |
| locked | bool | is the overlay now locked or unlocked |

## Example: Unlock the Overlay

```cs
var discord = new Discord.Discord(clientId, Discord.CreateFlags.Default);

var overlayManager = discord.GetOverlayManager();

overlayManager.OnLocked += locked =>
{
  Console.WriteLine("Overlay Locked: {0}", locked);
};
overlayManager.SetLocked(false);
```

## Example: Activate Overlay Invite Modal

```cs
var discord = new Discord.Discord(clientId, Discord.CreateFlags.Default);
var overlayManager = discord.GetOverlayManager();

// Invite users to join your game
overlayManager.OpenActivityInvite(ActivityActionType.Join, (result) =>
{
  Console.WriteLine("Overlay is now open!");
})
```

And that invite modal looks like this!

![](overlay-invite.gif)