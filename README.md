# GodotMods

GodotMods is a Godot addon that automatically loads mod PCK files from mod folders at startup.

Its goal is to remove the manual patch-apply step for compatible games. Instead of merging a patch into the base game, you can drop a GodotDelta-built mod PCK into a mods folder and let the game load it at runtime.

## Relationship with GodotDelta

GodotMods is designed to work alongside [GodotDelta](https://github.com/wheedo07/GodotDelta).

The intended split is:
- `GodotDelta`: build mod or patch PCK files from a Godot project
- `GodotMods`: load those PCK files automatically from mod folders at game startup

That means GodotDelta handles the mod package creation side, while GodotMods handles the runtime loading side.

Together, they enable a simpler workflow:
- build a mod PCK with GodotDelta
- distribute that `.pck` file
- place it in `user://mods` or `<game_directory>/mods`
- launch the game and let GodotMods load it automatically

This avoids requiring users to manually apply a patch into the original game for every mod update.

## What it does

At startup, GodotMods looks for `.pck` files in these locations:
- `user://mods`
- `<game_directory>/mods`

Each `.pck` file found there is loaded with:
- `ProjectSettings.load_resource_pack(pck_path, true)`

Files are sorted before loading, so load order is deterministic within each folder.

## Intended workflow

1. Create a mod project with GodotDelta-compatible resources.
2. Build a mod PCK from that project.
3. Put the resulting `.pck` file into one of the mods folders.
4. Launch the game.

No separate patch-apply step is required for this workflow.

## Recommended use with GodotDelta

GodotMods is intended to pair with GodotDelta.

Typical flow:
- build a mod PCK with GodotDelta
- distribute that `.pck` file as the mod
- let users place it in `user://mods` or `<game_directory>/mods`
- let GodotMods load it automatically at startup

This is useful when you want a drop-in mod workflow instead of replacing or rewriting the base game pack.

## Installation

Copy the addon into your project:
- `addons/GodotMods`

Then enable the plugin in Godot:
- `Project > Project Settings > Plugins > GodotMods > Enable`

The plugin registers an autoload loader that scans mod directories when the game starts.

## Runtime locations

### `user://mods`

Per-user mod folder managed through Godot's `user://` storage.

### `<game_directory>/mods`

A `mods` folder next to the game executable.

## Load behavior

- Only `.pck` files are loaded.
- Files are loaded in sorted filename order.
- Loading uses `replace_files=true`, so later packs can override earlier resources.