# Anti-hack tables

| File | What it controls |
|---|---|
| `Hack/HackPacketCheck.txt` | Which network packets the client may send, whether they must be encrypted, and how many per time window |
| `Hack/HackSkillCheck.txt` | How many casts of a skill are allowed per time window, by attack speed |
| `Hack/Dec1.dat`, `Hack/Enc2.dat` | Packet encryption keys — **do not edit** |

The switches that turn these checks on and decide the punishment are in `GameServerInfo - Common.dat` — see [Hack Speed Skill Settings](../gameserver/common.md).

---

## HackPacketCheck.txt

### What it does

Every packet the game client sends has a **head code** (and many have a **sub-code**). This table is a whitelist: for each packet it says if it is allowed, if it must arrive encrypted, and how fast it may be sent. A packet that is **not in the table** disconnects the client.

### Where it lives

`MuServer/Data/Hack/HackPacketCheck.txt`

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `Index` | 0–255 | Packet head code. |
| `Value` | 0–255 or `*` | Sub-code. `*` = the line covers every sub-code of this head. When a head has lines with specific sub-codes, only those sub-codes are allowed. |
| `Encrypt` | 0 / 1 / `*` | `1` = the packet must arrive encrypted; `0` = must arrive plain; `*` = either. A mismatch disconnects the client. |
| `MaxDelay` | ms | Length of the counting window. `0` = no flood check. |
| `MinCount` | number | Packets allowed inside the window. Extra packets are **ignored** and logged. `0` = no limit. |
| `MaxCount` | number | If the count passes this number inside the window, the client is **disconnected**. `0` = never disconnect for flood. |

### Example (real lines)

```
//Index   Value   Encrypt   MaxDelay   MinCount   MaxCount
3         *       1         0          0          0
24        *       0         2000       10         15
77        0       1         0          0          0
77        15      0         0          0          0
243       *       *         0          0          0
```

- Packet `3`: any sub-code, must be encrypted, no flood limit.
- Packet `24`: in any 2-second window, more than 10 are ignored, more than 15 disconnect.
- Packet `77`: only the listed sub-codes are accepted; sub-code 0 must be encrypted, 15 must be plain.

### Rules & pitfalls

{% hint style="danger" %}
Do **not** delete lines. A missing packet code makes every client that sends it disconnect ("Packet unknown error" in the hack log). Only tighten `MaxDelay`/`MinCount`/`MaxCount` after watching the hack log.
{% endhint %}

- Updating the client to a version that sends new packets requires adding those codes here.
- Too strict flood limits disconnect players with lag spikes (packets arrive in bursts).

---

## HackSkillCheck.txt

### What it does

Detects skill speed hacks: limits how many times a skill may be cast inside a time window, with different limits for different attack speeds.

The check only runs when `SpeedHackSkillEnable = 1`. What happens to a cheater (log, disconnect, block attacks, ban) is `SpeedHackSkillType` in `GameServerInfo - Common.dat`.

### Where it lives

`MuServer/Data/Hack/HackSkillCheck.txt`

### Syntax

| Column | Values | Meaning |
|---|---|---|
| `SkillIndex` | skill id | Skill to watch (from `Skill.txt`). |
| `MinSpeed` | number | Lowest attack speed this line applies to. |
| `MaxSpeed` | number or `*` | Highest attack speed (`*` = no upper limit). |
| `MaxDelay` | ms | Length of the counting window. |
| `MaxCount` | number | Casts allowed inside the window. More = speed hack detected. |
| `Comment` | `// text` | Free text. |

### Example (real lines)

```
//SkillIndex  	MinSpeed  MaxSpeed  MaxDelay     MaxCount   Comment
230       	000        125        1000       30         //Red Storm
230       	1001       *          1000       30         //Red Storm
end
```

Red Storm (230): at most 30 casts per second, at every attack speed.

### Rules & pitfalls

- Speed ranges of one skill must not overlap; the first matching line is used.
- Skills not listed are not checked.
- Test new limits with `SpeedHackSkillType = 0` (log only) before punishing.

---

## Dec1.dat and Enc2.dat

Binary encryption keys used for the packet encryption between client and server. **Do not edit or replace them** unless you received a matching pair together with a matching client: a mismatch disconnects every player. If either file is missing or broken, the GameServer writes a red `Hack\Enc2.dat` / `Hack\Dec1.dat` line in the log and encrypted packets stop working (players can't play).

---

## Edit in the panel

No dedicated visual editor. Use the [File editor](../../dashboard/file-editor.md) for the two `.txt` files.

## Reload module

```
/reload hack
```

Reloads the hack settings of `GameServerInfo - Common.dat`, `HackPacketCheck.txt`, `HackSkillCheck.txt` and the key files.
