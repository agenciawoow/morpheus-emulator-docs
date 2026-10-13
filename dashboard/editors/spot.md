# Spot editor

**Place monsters and NPCs over the server's real map.** The background is the collision grid the GameServer uses, so you can tell at a glance whether a spot landed on a wall or inside a safe zone.

For the spot file format, see the MuServer reference: [Monsters](../../muserver/data/monsters.md).

## Opening a map

1. Go to **Editors → Spot editor** (**Loading the server catalog…**).
2. In **Map**, pick the spot file. Files are grouped: **Maps**, **Events**, **Invasions**, **Kanturu (event)**.
3. Under the selector you see `map N`, which GameServer serves it (`served by GameServer …`) and **· no terrain file** if the map has no terrain (then the background is blank).

{% hint style="warning" %}
Files listed as **(ignored by the server)** or the folder warning **This folder is not read by any GameServer** are not loaded by the game. A file whose name doesn't follow the `NNN - Name.txt` pattern is ignored entirely. Castle Siege spots are edited in the normal map file of Castle Siege, not in the old `MonsterSetBaseCS` folder.
{% endhint %}

## Reading the map

The canvas shows each tile coloured by its terrain:

| Colour legend | Meaning |
|---|---|
| **walkable** | Monsters can spawn and walk here. |
| **blocked** | Walls and obstacles. |
| **safezone** | Towns and safe areas; normal monsters never spawn here. |
| **water** | Water tiles. |

Hover a tile to see its coordinates and type. Use **Zoom** to enlarge. Existing spots are drawn as markers (points) and rectangles (areas). **Click a spot to select it.**

## Spot sections

A spot file is split into sections. Pick the section with the tabs above the list (each shows its `N spots` count):

| Section | What it does | Use for |
|---|---|---|
| **Fixed point** | Spawns exactly at the coordinate, always 1. Terrain is not checked, so it works inside town safe zones. | NPCs, shopkeepers, guards. |
| **Area** | You draw a rectangle and say how many spawn inside it. The server picks free positions, rejecting blocked, no-ground and safe-zone tiles. | Populating maps with mobs to hunt. |
| **Scattered point** | Like the fixed point, but each spawn is spread ±3 tiles around it, respecting collision. | A handful of mobs near a point. |
| **Invasion** | Same as Area plus an invasion group. Only spawns when the invasion event runs. | Invasions. |
| **Event** | Fixed coordinates created by events (Blood Castle, Chaos Castle, Devil Square, Imperial Guardian). | Event monsters. |

{% hint style="info" %}
Open **What is the difference between a fixed point and an area?** in the editor for the full explanation. Sections marked as not spawned by the normal world spawner (**does not spawn on its own**) only matter when their event runs.
{% endhint %}

## Adding monsters

1. Select the section (for example **Area**).
2. Pick the monster or NPC with **Search for a monster or NPC…** (filter **All**, **Monsters**, **NPCs**; each shows `lv N`).
3. Click **Add monster**. The editor enters placing mode: **Placing in &lt;section&gt;**.
4. Set **Qty** (for areas).
5. Place on the map:
   - **Area** sections: **drag on the map to create as many areas as you want** (the hint shows `area 12 × 8 tiles — release to create`).
   - **Fixed / scattered** sections: **click on the map as many times as you want**.
6. The counter shows `N added`. Click **Done** or press **Esc** to exit placing mode.

## Editing a spot

Click a marker to open its properties (or click a free tile to add one to the current section).

| Property | Meaning |
|---|---|
| **Monster / NPC** | The monster, with `level N · HP` from the server monster list. NPCs show *does not attack; use Fixed point with radius 0 to keep it still*. |
| **Coordinate** / **Area (corner to corner)** | X/Y of the point, or from X/Y **to X** / **to Y** for areas. |
| **Facing** | One of 8 directions or **Random** (*The server picks one of the 8 directions at load time.*). |
| **Chase radius** | In tiles, measured from the spawn point. **0 = stays put** (that is how an NPC stops walking). |
| **Amount** | Areas only: how many monsters spawn inside the rectangle. |
| **Invasion group** | Invasion section only: matches the group in the invasion manager. |
| **Comment** | Free note saved in the file. |

Click **Remove this spot** to delete it.

{% hint style="info" %}
**Amount on a fixed point?** A fixed-point spot always spawns **1** monster — the file format has no amount field there. For several monsters, use the **Area** section.
{% endhint %}

## Warnings

The editor checks every spot against the terrain:

| Warning | Meaning |
|---|---|
| **monster N does not exist in Monster.txt — nothing will spawn** | Invalid monster index. |
| **the whole area is blocked/safezone — the server tries 100 times and gives up** | No valid tile in the rectangle. |
| **no free tile within ±3 of (x, y) — the monster will not spawn** | Scattered point surrounded by walls. |
| **tile (x, y) is blocked — it still spawns, but double-check the coordinate** | Fixed point on a non-walkable tile. |

## Saving and applying

1. Click **Save**.
2. On the [Dashboard](../server-dashboard.md), use **Reload → Reload Monster** on each GameServer serving that map. Monsters are removed and respawned with the new spots — players are not disconnected.

{% hint style="warning" %}
Switching maps with unsaved changes discards them (*You have unsaved spots in this file. Switching maps will lose them.*).
{% endhint %}
