# Raklion

Continuous boss cycle in Raklion: players destroy the spider eggs in the hatchery, then fight Selupan behind a closing door.

## What it is

* In the Raklion hatchery, spider eggs spawn. When half of them are destroyed a notice warns everybody. When all are destroyed, **Selupan** appears after a short delay.
* The boss zone stays open for a while, then the door closes and only the players inside can fight.
* Selupan changes attack pattern as his HP drops.
* When Selupan dies, or every player inside dies, the zone reopens after a delay and the cycle starts again with new eggs.

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServerInfo - Event.dat` | `RaklionEvent` | `0` | `1` = the Raklion cycle runs |

## Files & syntax

`Data/Event/Raklion.dat`, 4 sections, one row each. All delays are in **seconds**.

### Section 0 - Delays

| Column | Meaning | Values |
|---|---|---|
| AppearanceDelay | Seconds between the last egg destroyed and Selupan appearing | shipped `5` |
| BossZoneCloseDelay | Seconds the boss zone stays open after Selupan appears; then the door closes | shipped `300` |
| BossZoneOpenDelay | Seconds after the fight ends (Selupan dead or everybody dead) before the zone reopens and a new cycle starts | shipped `300` |

### Section 1 - Eggs

| Column | Meaning | Values |
|---|---|---|
| BossEggHalf | When this many eggs or fewer remain, the "half destroyed" notice is sent | shipped `5` |
| BossEggMax | Maximum number of eggs spawned | shipped `15` |

### Section 2 - Selupan skill delay

| Column | Meaning | Values |
|---|---|---|
| SkillDelay | Milliseconds between two Selupan skills | shipped `1500` |

### Section 3 - Selupan patterns

| Column | Meaning | Values |
|---|---|---|
| Pattern1 | Selupan stays in pattern 1 while his HP is above this % | shipped `80` |
| Pattern2 | Pattern 2 while HP is above this % | shipped `60` |
| Pattern3 | Pattern 3 while HP is above this % | shipped `50` |
| Pattern4 | Pattern 4 while HP is above this % | shipped `40` |
| Pattern5 | Pattern 5 while HP is above this % | shipped `20` |
| Pattern6 | Pattern 6 while HP is above this %; below it, the last pattern | shipped `10` |

Each pattern adds stronger skills (poison, ice storm, ice strike, freeze, teleport, summons). Values must go down from left to right.

## Example

```
0
//AppearanceDelay   BossZoneCloseDelay   BossZoneOpenDelay
10                  180                  600     // boss after 10 s, door closes after 3 min, next cycle 10 min later
end

1
//BossEggHalf   BossEggMax
10              20
end

2
//SkillDelay
1200                                             // a skill every 1.2 s
end

3
//Pattern1   Pattern2   Pattern3   Pattern4   Pattern5   Pattern6
90           70         50         35         20         10
end
```

## Schedule

Raklion has no clock schedule and is not listed in [Schedule syntax](schedule-syntax.md). The cycle restarts by itself after each fight.

## Rewards

There is no reward table. Selupan's drop comes from the item bag matched by his monster class (459): index 67 in `Data/EventItemBagManager.txt`, file `Data/EventItemBag/067 - Selupan.txt`.

## Commands related

There is no command to start Raklion.

| Command | Who | What |
|---|---|---|
| `/reload event` | Admin | Re-reads `Raklion.dat` and restarts the cycle |

## Pitfalls

* The event ships disabled (`RaklionEvent = 0`).
* Section 0 values are seconds, section 2 is milliseconds.
* Pattern values that do not go down from left to right make Selupan skip patterns.
* A very short BossZoneCloseDelay closes the door before players reach Selupan.

## Reload module

`/reload event` re-reads `GameServerInfo - Event.dat` and `Raklion.dat`, and forces Raklion to its end state so a fresh cycle starts.

{% hint style="warning" %}
A Selupan fight that is running when you reload is ended. Reload when nobody is fighting the boss.
{% endhint %}
