# Exchange Coin

Chat commands that convert one coin into another at a rate you set, for example 10 WCoinC into 1000 GoblinPoint.

## What it is

The player types a command you created, such as `/wctogp`. The server checks that they have enough of the coin being paid, takes it and credits the coin being bought, then shows `[ExchangeCoin] Exchange completed successfully!`. If the balance is too low, the player sees how much is needed, for example `[ExchangeCoin] Required: 10 WC!`.

The same commands can be attached to an NPC (see [NPC Customs](npc-customs.md)), in which case the NPC also says the result.

## Enable it

There is no on/off key. Every row in `CustomExchangeCoin.txt` is a working command. In the shipped file both example rows are commented out, so no exchange is active.

Messages come from `Data/Lang`, translated per player language:

| Lang index | English text |
|---|---|
| 877 | `[ExchangeCoin] Required: %d WC!` |
| 878 | `[ExchangeCoin] Required: %d WP!` |
| 879 | `[ExchangeCoin] Required: %d GP!` |
| 880 | `[ExchangeCoin] Exchange completed successfully!` |

## Files & syntax

File: `Data/Custom/CustomExchangeCoin.txt`. One row per command, then `end`. No sections.

| Column | Meaning | Values |
|---|---|---|
| `Index` | Row number | Any unique number |
| `Command` | Command the player types, including `/` | Text in quotes, up to 31 characters. Not case sensitive |
| `RequireCoin1` | WCoinC taken | Number, `*` or `0` = none |
| `RequireCoin2` | WCoinP taken | Number, `*` or `0` = none |
| `RequireCoin3` | GoblinPoint taken | Number, `*` or `0` = none |
| `RewardCoin1` | WCoinC given | Number, `*` or `0` = none |
| `RewardCoin2` | WCoinP given | Number, `*` or `0` = none |
| `RewardCoin3` | GoblinPoint given | Number, `*` or `0` = none |

## Example

```
//	#Index 	#Command	#RequireCoin1	#RequireCoin2 	#RequireCoin3	#RewardCoin1	#RewardCoin2	#RewardCoin3
	1	"/wctogp"	10		*		*		*		*		1000	// 10 WCoinC -> 1000 GoblinPoint
	2	"/wptowc"	*		20		*		10		*		*	// 20 WCoinP -> 10 WCoinC
end
```

## Rewards

The `RewardCoin` columns are credited to the account in the same operation that debits the `RequireCoin` columns. Coins: 1 = WCoinC, 2 = WCoinP, 3 = GoblinPoint.

## Commands related

| Command | Who | What |
|---|---|---|
| Any command you add here | Player | Runs that exchange once. |
| `/reload custom` | Admin | Re-reads the file. |

## Pitfalls

* The command name must not exist in `Data/Command.txt`: those commands are checked first, and yours would never run.
* There is no confirmation and no cooldown. One command = one exchange. Choose rates that cannot be looped for profit (for example A -> B -> A ending with more than you started).
* The balance check uses the coin values the server last read for the player. If you add coins from the website while the player is online, have them reopen the cash shop or relog before exchanging.
* Keep the command under 32 characters: a longer value can stop the GameServer while the file loads.

## Reload module

`/reload custom` re-reads `Data/Custom/CustomExchangeCoin.txt`. Changes apply to the next command typed. No client change is needed.
