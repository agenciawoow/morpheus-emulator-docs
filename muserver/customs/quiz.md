# Quiz

A scheduled question-and-answer event: the server posts a question (or a coupon code) to everyone, and the first player to answer correctly with `/resp` wins coins.

## What it is

At the scheduled time the server announces the event, picks one random question from your list and broadcasts it ("[COD EVENT] Question: ..."). The question is repeated every minute while the event is open. The first player who types `/resp <answer>` with the right answer wins: the server announces the winner and the answer, gives the coins and plays fireworks. Wrong answers get a private "Wrong Answer!" notice. If nobody answers before the time runs out, "There was no winner!" is announced. The next quiz time also appears in the Event Time window (H key).

The shipped configuration is a "coupon" quiz: the question text already shows the code (`Use /resp <code> and win GP: LORENCIA50`) and the first player to type it wins.

## Enable it

| File | Key | Default | Meaning |
|---|---|---|---|
| `GameServer/Data/GameServerInfo - Custom.dat` | `CustomQuizSwitch` | `1` | `1` = scheduled quizzes run and `/resp` works. `0` = disabled. |
| `Data/Command.txt` | row `53 "/resp"` | enabled, GameMaster `0` | The answer command. |
| `Data/Command.txt` | row `77 "/startquiz"` | enabled, GameMaster `2` | Admin command to start a quiz. |

## Files & syntax

File: `Data/Custom/CustomQuiz.txt`. Three sections, each closed by `end`. Up to 24 quiz schedules (index `0..23`).

### Section 0 - Schedule

| Column | Meaning | Values |
|---|---|---|
| Index | Quiz index | `0..23` |
| Year, Month, Day | Date | `*` = any |
| DoW | Day of week | `*` = any, `1` = Sunday ... `7` = Saturday |
| Hour, Minute, Second | Start time | |

### Section 1 - Quiz rules

| Column | Meaning | Values |
|---|---|---|
| Index | Quiz index | `0..23` |
| Name | Name shown in the notices, in quotes | `"COD EVENT"` |
| AlarmTime | Minutes of "starts in N minute(s)" notices before the start | `0` = no countdown |
| EventTime | Minutes the question stays open | e.g. `3` |

### Section 2 - Questions

| Column | Meaning | Values |
|---|---|---|
| ID | Question id | must be `0, 1, 2...` with **no gaps** |
| WC | WCoinC prize | |
| WP | WCoinP prize | |
| GP | GoblinPoint prize | |
| Question | Text broadcast to players, in quotes | |
| Answer | Correct answer, in quotes | compared case-insensitively against everything typed after `/resp` |

## Example

```
0
//Index Year Month Day DoW Hour Minute Second
0       *    *     *   *   12   0      0       // every day at 12:00
0       *    *     *   *   20   0      0       // and at 20:00
end

1
//Index Name           AlarmTime EventTime
0       "MU Quiz"      5         3
end

2
//ID WC WP GP  Question                                   Answer
  0  0  0  50  "What is the capital of the MU continent?" "lorencia"
  1  0  0  50  "Type the code: DEVIAS50"                  "DEVIAS50"
end
```

## Schedule

Section 0 uses the shared schedule format; see [schedule syntax](../events/schedule-syntax.md). As shipped: index `0` runs at minute 57 of every hour, 3 minutes long, no countdown.

`/startquiz` adds a one-time start for index `0` two minutes from now (it does not remove the normal schedule).

## Rewards

The winner receives the WC / WP / GP values of the question that was asked (Section 2), added to the account.

## Commands related

| Command | Who | What |
|---|---|---|
| `/resp <answer>` | Player | Answers the current question. |
| `/startquiz` | Admin (GameMaster `2`) | Schedules quiz index `0` to start in 2 minutes. |

## Pitfalls

- Question IDs must start at `0` and be continuous. The server draws a random number between `0` and the number of questions minus one; a missing ID makes that draw fail silently (logged as an error, no quiz that round).
- Never leave Section 2 empty while a schedule is active: the draw divides by the number of questions, and zero questions can crash the GameServer at quiz time.
- The Lang text for the question notice says "Use /answer to respond", but the shipped command is `/resp`. Either rename the command in `Data/Command.txt` or edit the Lang message (index 746) to match.
- The answer must match exactly (only case is ignored): extra spaces or accents make it wrong. Avoid accented answers.
- Only one quiz can be active at a time; overlapping schedules share the same question state.
- `/startquiz` near 23:59 may compute an invalid time; use it at other times.

## Reload module

- `/reload custom` re-reads `CustomQuiz.txt` and `CustomQuizSwitch`, but does not recompute the next quiz time.
- `/reload event` re-initialises the quiz schedule (and every other scheduled event) and applies the switch.

Recommended order after editing: `/reload custom`, then `/reload event`, outside a running quiz.
