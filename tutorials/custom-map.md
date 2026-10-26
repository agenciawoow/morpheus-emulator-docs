# Adding a custom map

This tutorial adds a new hunting map, **Crystal Valley**, as map number **140**: players reach it with `/move` or the **M** window, it has its own rules, monsters, respawn point, name, title image and map markers.

A map is made of files on **both** sides. The server needs to know where players can walk and what lives there; the client needs the 3D world to draw.

| Part | Side | File(s) | Edited in | Applied with |
|---|---|---|---|---|
| Walkable tiles, safe zones, walls | Server | `Data/Terrain/terrain141.att` | Added to the folder (binary) | `/reload terrain` |
| Map rules (PK, experience, drops, helper, respawn) | Server | `Data/MapManager.txt` | [File editor](../dashboard/file-editor.md) | `/reload map` |
| Arrival / respawn point | Server | `Data/Move/Gate.txt` | File editor | `/reload move` |
| Warp entry (`/move`, M window) | Server | `Data/Move/Move.txt` | File editor | `/reload move` |
| Monsters and NPCs | Server | `Data/MonsterSetBase/140 - Crystal Valley.txt` | [Spot editor](../dashboard/editors/spot.md) after the file exists | `/reload monster` |
| Which GameServer hosts it (multi-server only) | Server | `Data/MapServerInfo.dat` | File editor | `/reload map` |
| 3D world (terrain, objects, lighting) | Client | `Data/World141/`, `Data/Object141/` | Game client files | Included in the client you distribute |
| Map behavior, name, title image, music | Client | `CustomWorld.txt` | **Client settings → Custom loaders → Worlds/Others** | Publish update |
| Map name text | Client | **Text.bmd** | **Client Hub → Client Files** | Publish update |
| M window entry | Client | **MoveReq** | **Client Hub → Client Files** | Publish update |
| Markers on the full map / minimap (optional) | Client | `CustomFullMap.txt` | **Custom loaders → Worlds/Others** | Publish update |

References: [Maps](../muserver/data/maps.md), [Gates, warps and summon areas](../muserver/data/move.md), [Monsters and spawns](../muserver/data/monsters.md), [Client custom files](../muserver/client/custom-files.md#customworld), [Spot editor](../dashboard/editors/spot.md).

## Where map numbers and folder numbers come from

Everything about a map is keyed by its **map number**. Files that live in folders or are named by number use **map number + 1**:

| For map 140 | Name |
|---|---|
| Server terrain | `terrain141.att` |
| Client world folder | `Data/World141/` with `EncTerrain141.map`, `EncTerrain141.att`, `EncTerrain141.obj`, `TerrainHeight.OZB`, `TerrainLight.OZJ` and the ground textures |
| Client objects folder | `Data/Object141/` (trees, rocks, buildings of the map and their textures) |
| Client minimap image (optional) | `Data/Interface/HUD/NaviMap/Navimap141.ozt` |
| Spot file | `140 - Crystal Valley.txt` (here the **map number** itself) |

Pick a map number that is not used by any shipped map on either side. The shipped server uses numbers up to about `133` (check `Data/MapManager.txt` and `Data/Terrain/`), and the client custom file `CustomWorld.txt` lists the custom maps it already knows (up to `134`). Numbers above `81` are "custom maps" for the client and **must** have a `CustomWorld.txt` row.

---

{% stepper %}
{% step %}
### Get the map files

A custom map comes from a map maker or another client version as a set of files:

- the **client** world: the `World` folder (terrain mapping, height, light, attribute and object placement files plus the ground textures) and the `Object` folder (3D objects of the map);
- the **server** attribute file (`terrainN.att`), which must match the client attribute file of the same map exactly.

Rename the folders and files to your map number (`World141`, `Object141`, `EncTerrain141.*`, `terrain141.att`).

{% hint style="danger" %}
The server `.att` and the client world must come from the **same** map. If they differ, players walk through walls on their screen while the server pulls them back, or they can't enter areas they see as open. `.att` files are binary: never edit them by hand.
{% endhint %}
{% endstep %}

{% step %}
### Install the server terrain

Copy `terrain141.att` into the server's `Data/Terrain/` folder (the File editor doesn't upload binary files; add it to the folder directly). Then:

```
/reload terrain
```

If no `terrain141.att` exists, the server silently uses the terrain of the previous map number, which is never what you want.
{% endstep %}

{% step %}
### Add the map rules (MapManager.txt)

In `Data/MapManager.txt`, add a line before `end`:

```
//Index  NonPK  ViewRange  ExperienceRate  ItemDropRate  ExcItemDropRate  SetItemDropRate  SocketItemDrop  HelperEnable  GensBattle  CustomAttack  CustomStore  CustomPick  PkDropItem  DeathGate  Comment
140      *      10         200             120           100              50               0               1             0           1             0            1           0           610        "Crystal Valley"
```

| Value | Column | Effect |
|---|---|---|
| `*` | NonPK | Use the server's global PK rule. `1` = non-PK map, `2` = free PvP. |
| `200` | ExperienceRate | Double experience here. |
| `120` | ItemDropRate | 120% of the normal item drop chance. |
| `100` / `50` | ExcItemDropRate / SetItemDropRate | Per **million**: 0.01% excellent, 0.005% ancient per kill (monsters level 25+). |
| `1` | HelperEnable | MU Helper allowed. |
| `1` / `0` / `1` | CustomAttack / CustomStore / CustomPick | `/attack` allowed, `/store` not, `/pick` allowed. |
| `610` | DeathGate | Players who die here respawn at gate 610 (next step). Use `17` to send them to Lorencia instead. |
| `"Crystal Valley"` | Comment | Required, quoted. |

Every line needs all 16 columns. Apply with `/reload map`.
{% endstep %}

{% step %}
### Add the arrival gate (Gate.txt)

In `Data/Move/Gate.txt`, add a **respawn/warp target** gate (Flag `0`) on a walkable, ideally safe area of the new map:

```
//Index   Flag   Map   X      Y      TX      TY      TargetGate   TargetDir   MinLevel   MaxLevel   MinReset   MaxReset   AccountLevel
610       0      140   120    120    125     125     0            1           200        1400       *          *          0
```

- `610` = a free gate number (the shipped file uses numbers up to `604`).
- Players arrive at a random tile inside the rectangle 120,120 – 125,125.
- `200` = minimum level to be sent here.

Open the map in the [Spot editor](../dashboard/editors/spot.md) (after the spot file exists, below) to read coordinates on the real terrain: hover a tile to see its X/Y and whether it is walkable or a safe zone.

{% hint style="info" %}
A Flag `0` gate needs nothing on the client. **Walk-in** entrances (Flag `1`/`2`, stepping on stairs or a portal) also need the client's own gate table to match, which the Client Hub does not edit. Use warps (`/move`, the M window, NPCs, death gate) for custom maps, or ask support if you need a walk-in portal.
{% endhint %}
{% endstep %}

{% step %}
### Add the warp (Move.txt)

In `Data/Move/Move.txt`, before `end`:

```
//Index  MainName   SubName    Money   MinLvl  MaxLvl  MinReset  MaxReset  AccLvl  Gate
62       "Crystal"  "Crystal"  20000   200     -1      -1        -1        0       610
```

- `62` = a free warp number (the shipped list ends at `61`).
- `"Crystal"` = what players type: `/move Crystal`. Avoid spaces in `MainName`.
- `20000` Zen, level 200+, any reset, any account level, arrives at gate `610`.

Apply both files with:

```
/reload move
```

`/move Crystal` works now. The **M** window entry is a client change (step 10).
{% endstep %}

{% step %}
### Create the spot file and place monsters

Spawns for map 140 must be in a file whose name starts with `140 - `. The panel doesn't create new files, so add the file to `Data/MonsterSetBase/` directly (copy any small spot file and rename it):

```
Data/MonsterSetBase/140 - Crystal Valley.txt
```

Start content:

```
1
//Monster  MapNumber  Range  BeginPosX  BeginPosY  EndPosX  EndPosY  Direction  Quantity  Comment
46         140        6      100        150        130      180      -1         15        // Vepar
52         140        8      150        100        190      130      -1         10        // Silver Valkyrie
end
```

Then open **Editors → Spot editor**, pick `140 - Crystal Valley` under **Maps**, and place the rest by clicking and dragging on the real terrain (the editor warns about blocked tiles and safe zones). Click **Save**, then **Reload → Reload Monster** on the GameServer.

{% hint style="warning" %}
The map number in the **file name** wins: every line of `140 - ….txt` spawns on map 140, whatever its `MapNumber` column says. A file named `140-Crystal Valley.txt` (no spaces around the dash) is ignored.
{% endhint %}
{% endstep %}

{% step %}
### Multi-server setups: who hosts the map

Skip this step if you run one GameServer (plus the Castle Siege server).

`Data/MapServerInfo.dat` says which GameServer hosts which map. A server with `InitSetVal 1` hosts every map that section 1 doesn't send elsewhere, so a new map is hosted automatically. To host map 140 on another server (code `1` here), add to section 1:

```
//ServerCode   NotMoveOption   NextMap   NextServerCode
0              0               140       1
1              1               140       -1
end
```

Monsters spawn only on the server that hosts the map. Apply with `/reload map`. See [Maps](../muserver/data/maps.md).
{% endstep %}

{% step %}
### Install the client world files

Put `Data/World141/` and `Data/Object141/` in the game client, and optionally the minimap image `Data/Interface/HUD/NaviMap/Navimap141.ozt` and the title image (next step). The `.att`, `.bmd` and texture files (`.ozj` / `.ozt` / `.tga`) can be uploaded in [Client Files → Data files](../dashboard/client-hub/client-files.md#data-files). The terrain `.map` and `.obj` files are not accepted there yet: add them to the client you distribute, or send them to support if they maintain your client package.
{% endstep %}

{% step %}
### Register the map on the client (CustomWorld.txt)

**Client Hub → Client settings → Custom loaders → Worlds/Others → CustomWorld.txt**:

```
//#Map  #Effect  #TextID  #Ext  #ImgName             #Mp3File
140     -1       4030     0     "crystalvalley.tga"  "None"
```

| # | Column | Value | Meaning |
|---|---|---|---|
| 1 | Map | `140` | Map number (above 81). |
| 2 | Effect | `-1` | Original map whose special behavior and ambient effects the map reuses: `91` Acheron, `95` Debenter, `100` Uruk Mountain, `110` Nars, `112` Ferea, `113` Nixie Lake. `-1` = a plain map. Use the value of the map your files came from when it is one of these. |
| 3 | TextID | `4030` | Key in **Text.bmd** with the map name, used wherever the client shows the map's name. `-1` = default text. |
| 4 | Ext | `0` | `1` = the map's height file is in the **extended** format used by maps from newer seasons. If the ground looks flat or spiky, flip this value. |
| 5 | ImgName | `"crystalvalley.tga"` | Title image shown when entering the map, from `Data/Local/<language>/ImgsMapName/` (one copy per client language, for example `Eng`). |
| 6 | Mp3File | `"None"` | Background music, for example `"data\\music\\Devias.mp3"`, or `"None"` for no music. |

Click **Save**.
{% endstep %}

{% step %}
### Add the name and the M window entry

1. **Client Hub → Client Files → Text.bmd**: filter by key `4030`. If it doesn't exist, **+ entry** with key `4030` and text `Crystal Valley`. Fill every language and **save**. (The shipped custom maps use keys `4000`–`4024`; use any key not in use.)
2. **Client Hub → Client Files → MoveReq**: add a row with **No.** `62` (same as `Move.txt`), **Map** `Crystal`, **Min level** 200, **Max level** *no limit*, **Zen** 20000, **Gate** 610. Do it in every language file.

*The real `/move` is validated by the server (`Move.txt`/`Gate.txt`); keep the same numbers and levels on both sides.*
{% endstep %}

{% step %}
### Optional: markers on the full map

`CustomFullMap.txt` (**Custom loaders → Worlds/Others**) puts labelled icons on the full map (Tab) and minimap. It only works with the client setting **`CustomFullMapSwitch` = 1** (Client settings, **Custom** tab, map markers).

```
//  Index  MapNum  Type  CordX  CordY  FixName  MiniMap  Name
    900    140     2     122    122    1        1        "Arrival"
    901    140     3     115    165    0        1        "Vepar spot"
    902    140     3     170    115    0        1        "Valkyrie spot"
end
```

`Type`: 1 NPC, 2 Gate, 3 Spot, 4 Boss, 5 Event, 6 Quest, 7 Vault, 8 Point. `FixName 1` keeps the name always visible; `MiniMap 1` also shows the icon on the minimap. Use unused `Index` numbers.
{% endstep %}

{% step %}
### Publish the client update

1. **Client settings → Publish update → Prepare**.
2. **Publish updates** → check the client configuration, **Text.bmd** and **MoveReq** → **Publish N files** → **Deploy**.
3. Restart the game through the launcher.

See [Publishing updates](../dashboard/client-hub/publishing-updates.md).
{% endstep %}

{% step %}
### Test in game

1. On a level 200+ character with 20,000 Zen: `/move Crystal`, or pick **Crystal** in the **M** window.
2. Or, on a Game Master character: `/gmmove <YourName> 140 122 122`.

Check:

- the world draws correctly (ground, objects) and the title image and name appear;
- you can walk where the ground looks walkable and are blocked by walls;
- the monsters spawn in the areas you placed;
- dying sends you to gate 610 (or where your `DeathGate` points);
- experience and drop rates feel like your `MapManager.txt` line;
- the markers appear on the full map, if you added them.
{% endstep %}
{% endstepper %}

---

## Reload summary

| You changed | Reload |
|---|---|
| `Data/Terrain/terrain141.att` | `/reload terrain` (refused while Blood Castle, Chaos Castle and similar events run; `force` reloads anyway) |
| `MapManager.txt`, `MapServerInfo.dat` | `/reload map` |
| `Gate.txt`, `Move.txt` | `/reload move` |
| Spot file | `/reload monster` |
| Any client file | Publish the update; players restart |

On the Dashboard these are **Reload Terrain**, **Reload Map**, **Reload Move** and **Reload Monster**. Reload **every** GameServer that hosts the map. See [Services and reload](../dashboard/services-and-reload.md).

## Common mistakes

| Symptom | Cause | Fix |
|---|---|---|
| Black screen or crash when entering | Client world folder missing, wrongly numbered (`World140` instead of `World141`), or no `CustomWorld.txt` row. | Use map number + 1 for folders and files; add the row; publish. |
| Map draws but the ground is flat/spiky | Wrong `Ext` value. | Flip `Ext` between 0 and 1; publish. |
| Players walk through walls or rubber-band | Server `.att` doesn't match the client world, or `terrain141.att` is missing (the previous map's terrain is used). | Use the matching `.att`; `/reload terrain`. |
| `/move Crystal` says the map doesn't exist | `Move.txt` not reloaded, or `MainName` typed differently. | `/reload move`; type exactly the `MainName`. |
| Warp works but M window doesn't list it | MoveReq not updated or not published. | Add the MoveReq row in every language; publish. |
| No monsters | Spot file name wrong, map hosted by another GameServer, or `/reload monster` not done. | Name it `140 - Name.txt`; check `MapServerInfo.dat`; reload. |
| Monsters stand in walls or don't appear | Spots on blocked tiles or inside the safe zone. | Fix them in the Spot editor (it shows the warnings). |
| Map name shows a random text | `TextID` points to a key used by something else, or Text.bmd not saved in that language. | Use a free key; fill every language. |
| Dead players appear in Lorencia | `DeathGate` points to a gate that doesn't exist. | Create the gate or point to an existing one. |
| Changes on the client don't show | Saved but not published, or players didn't restart. | Publish and restart the game. |
