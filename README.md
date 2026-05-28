# Romestead Pelican Egg

A [Pelican Panel](https://pelican.dev/) egg for the
[Romestead](https://romestead.wiki.gg/wiki/Romestead_Dedicated_Server_Setup_Guide)
dedicated server (Steam App ID **4763510**, Linux, .NET 8).

## Install

In the Pelican admin UI, go to **Admin → Eggs → Import Egg** and use the **URL** tab with:

```
https://raw.githubusercontent.com/pbean/romestead-egg/main/egg-romestead.json
```

Or download [`egg-romestead.json`](./egg-romestead.json) and use the **File** tab.

## What it does

- Installs the Romestead Dedicated Server via SteamCMD (anonymous login).
- Runs the server with `dotnet Server.dll` on
  `ghcr.io/pelican-eggs/steamcmd:dotnet` (Debian + SteamCMD + .NET 8 runtime).
- Generates a default `config.json` on first install and rewrites
  `Port`, `Password`, `MaxPlayers`, `AutoStartWorldName`, and
  `AutoCreateWorldSize` on every boot from panel variables.
- Sends `stop` over stdin for clean shutdown.

## Variables exposed to the panel UI

| Variable          | Default     | Notes                                          |
| ----------------- | ----------- | ---------------------------------------------- |
| `WORLD_NAME`      | `auto`      | `AutoStartWorldName` in `config.json`.         |
| `WORLD_SIZE`      | `1`         | `0` = Small, `1` = Standard, `2` = Large (8 GB RAM). |
| `SERVER_PASSWORD` | `change-me` | Leave blank for no password.                   |
| `MAX_PLAYERS`     | `8`         | 8 GB RAM recommended for 5+ players.           |
| `AUTO_UPDATE`     | `1`         | Re-run SteamCMD on each boot.                  |
| `SRCDS_BETAID`    | empty       | Optional Steam beta branch.                    |
| `SRCDS_BETAPASS`  | empty       | Optional Steam beta password.                  |

The Steam App ID is locked to `4763510`.

## Resource recommendations

Per the Romestead wiki: 4 GB RAM for standard worlds with ≤ 4 players,
8 GB RAM for large worlds or 5+ players. The default port is **UDP 8050** but it will use your primary port allocation automatically.

## Tuning the ready-indicator

The egg's `config.startup.done` string is currently `"World loaded"` as a
conservative placeholder. If the panel never marks the server as "running"
after startup, replace that string with whatever line your console actually
prints when the world finishes loading.
