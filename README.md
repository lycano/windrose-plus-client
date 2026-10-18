# Windrose+ client

An in-game admin console for [Windrose](https://store.steampowered.com/app/3041230/) servers that run
[WindrosePlus](https://github.com/HumanGenome/WindrosePlus). It draws a console over the game, sends commands to the
server and shows the answers. No alt-tabbing into RCON or a web dashboard while you play.

> **Status:** in development, tested on our own servers. The client itself is not published yet. This repository
> shows what it looks like and how it works: **[open the interactive console](index.html)** (works offline in any
> browser; it is a copy of the console's UI, not the game).

## What the console does

- **F10** opens and closes it, in the main menu and in a world. Every other key stays typeable.
- **Command palette with subcommands.** Type `wp.` and the palette lists the server's commands. Commands with
  subcommands show `+N`. A space after such a command lists its subcommands, indented (`wp.give ` → `item`,
  `status`, `list items`, …). Tab cycles, the Right arrow or a click completes.
- **Syntax help** under the input line for the command or subcommand you are typing, with its own description.
- **Only what you may run.** The server sends each player the commands their role allows
  (`public < member < admin < dev`). Admins change roles and rules in game with `wp.perms`.
- **Server manager** (`/connect`): Direct-IP servers, favorites, stored passwords, join from the main menu or from a
  world.
- **Settings** (`/ui`): zoom 1 / 1.5 / 2 (pixel-exact fonts) and console height.
- **Updates over the game connection** (`/update`): the server offers a client version, the client checks its
  signature before it is loaded.

## Try the demo

Open [`index.html`](index.html), then:

1. Type `wp.` and look at the palette. Press Tab a few times.
2. Type `wp.give ` with a trailing space: the palette switches to the subcommands of `wp.give`.
3. Type `l`, then press the Right arrow: the line becomes `wp.give list items `, and the syntax bar shows its usage.
4. Switch the catalog (top right) between *offline*, *admin* and *public* to see what different roles get.
5. Try `/connect` and `/ui` for the server manager and the settings view. `/wp` goes back.

The demo's server commands are a snapshot of a WindrosePlus test server with our add-ons. Nothing you type leaves
your browser.

## Server side

The console talks to WindrosePlus through its command API. The parts it needs are proposed upstream as separate,
linked pull requests:

| Pull request | What it adds |
|---|---|
| Event bus | add-ons react to game events (`API.on`, `API.emit`) |
| Deferred command answers | a command can answer later (game-thread work), with a timeout |
| Config reload hook | add-ons pick up `windrose_plus.json` changes |
| Command API | add-ons list and run commands for other channels (like this console) |
| Subcommands | commands register subcommands with their own usage and description; `wp.help` shows a tree |
| Permissions | roles per account and per command for in-game channels |

The overview issue in WindrosePlus links them in order.

## Files

| Path | What it is |
|---|---|
| `index.html` | the rendered demo (template + a snapshot of the console's data) |
| `assets/console_board.html` | the template: the console's look and behaviour in HTML/JS, mirroring the client's ImGui code; its data goes in at `/*__STATE__*/` |

The demo loads two fonts from Google Fonts; apart from that it makes no network requests.
