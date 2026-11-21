# Miner options

**English** · [Português (Brasil)](../pt-BR/options.md) · [Back to README](../../README.md)

Options that work the same for every coin. Visual guide to the built-in web panel:
[Web Panel & Multi-Rig Hub](panel.md). Coin options are on each coin's page:
[Keryx](../../coins/keryx/docs/en/options.md), [Vela](../../coins/vela/README.md#vela-options),
[Proxima](../../coins/proxima/README.md#proxima-options), [NEB](../../coins/neb/README.md#neb-options),
[Numen](../../coins/numen/README.md#numen-options), [Vearl](../../coins/vearl/README.md#vearl-options).

## Where options go

| Platform | Where |
| --- | --- |
| HiveOS | Flight sheet, **Extra config arguments** |
| Linux | On the line in the coin's `start-<coin>.sh` |
| Windows | On the line in the coin's `start-<coin>.bat` |

Miner flags take one hyphen. Coin flags take two. They can sit in any order, on the same line.
A bare `--` from an older command line is still accepted and ignored.

```
-device 1,6 -stats 0.0.0.0:4060 --tier 2
└──── miner flags ─────────────┘ └ coin ┘
```

Paths are one word, with no quotes.

## Options

| Option | What it does | Default |
| --- | --- | --- |
| `-coin keryxhash` | Coin to mine: `keryxhash`, `vela`, `proxima`, `neb`, `numen` or `vearl`. Same as a positional coin name; when both are set, `-coin` wins | required |
| `-device 0,2,5` | Mine on these cards only, by the numbers the miner shows | `all` |
| `-pool host:port` | Pool to mine on: `host:port`, `stratum+tcp://host:port` (plain) or `stratum+ssl://host:port` (encrypted) | set by the launcher |
| `-node grpc://IP:PORT` | Your node, for solo mining | set by the launcher |
| `-wallet ADDRESS` | Payout address. `-address` is the same field; the last one wins | set by the launcher |
| `-worker NAME` | Rig name. Replaces the part of the wallet after the first `.` | |
| `-user ADDRESS.WORKER` | Full pool username. Prefer `-wallet` and `-worker` | set by the launcher |
| `-password x` | Pool password | `x` |
| `-stats ADDRESS:PORT` | Address of the stats API (`GET /stats`) | `127.0.0.1:4060` |
| `-panel ADDRESS:PORT` | Address of the panel, or `off` | `0.0.0.0:4455` |
| `-open` | Opens the panel in a window of its own and ends; the app menu entry runs it | |
| `-shortcut` | Writes the panel's app menu entry again and ends | |
| `-log-file PATH` | Where to write `log.txt` | `logs/log.txt` in the data folder |
| `-update manual` | Look for a new FCKHash version every 3 hours. `manual`: say so, and install it when you press Install in the panel; `auto`: install it by itself; `off`: never look | `manual` |

Two hyphens still work for these flags (`--pool`, `--device`). Coin flags do not: `--escrow-dir`
is a coin flag, `-escrow-dir` is rejected.

The stats API has no password. `0.0.0.0:4060` opens it to every machine that can reach the rig,
so keep it on your LAN.

The panel has no password either, and by default every machine on the rig's network can open it
(`http://RIG-IP:4455`). Over Tailscale, use the rig's `100.x` address. On a rig reachable from
the internet, pass `-panel 127.0.0.1:4455` or `-panel off`.

The panel's Control tab changes the coin, pool, wallet, the cards that mine, the coin's options,
auto OC and dual mining, and retunes a card; the miner starts again to apply each change. Anyone who can open the panel can change
them, the wallet too: on a network you share, close it as above. What the panel saves runs over
the start line (flight sheet or start file) until the start line changes: then the start line
wins and the panel's settings are cleared. A new FCKHash version keeps them.

Every 3 hours the miner asks FCKHash's public release page whether a newer version is out. It
sends nothing about the rig. When there is one, the log says so and the Control tab shows it with
its notes.

- On HiveOS, set the Installation URL the Control tab shows on your flight sheet and apply it.
- On Linux and Windows, press Install in the Control tab, or pick "Install by itself"
  (`-update auto`). The miner downloads the new version, checks it, and starts again with it when
  no AI request is running. Your start file, settings, history and data stay as they are. If the
  new version does not mine on the rig, the previous one comes back by itself and the panel says
  so; that version is not offered again.
- The miner's folder must be writable by the user that runs it. Otherwise install by hand:
  download the package and unpack it over the folder, keeping your start file.

`-update off` stops the check.

The Connected rigs tab adds another rig's panel by its address (`192.168.0.60:4455`, or its
Tailscale address or name). The panel then reads that rig every 5 seconds, keeps its history,
and shows it in every tab: pick it in the sidebar. A rig to be added needs its panel reachable
from this one.

The Logs tab's Export logs button saves one zip with the rig's logs, its cards, processor,
memory, board and disks, the system's recent errors and the last two days of readings. Send it
with a bug report. Passwords and tokens are taken out; the wallet and the pool stay.

## Overclock

The miner sets clocks and fans itself, on Linux and Windows, through the NVIDIA driver (NVML).

| Option | What it does |
| --- | --- |
| `-cclock 275` | Core clock offset in MHz. Negative lowers it: `-cclock -100` |
| `-mclock 1000` | Memory clock offset in MHz. Negative lowers it: `-mclock -500`. The memory clock the miner shows moves by half the value |
| `-lock-cclock 2610` | Locks the core clock at this MHz |
| `-lock-mclock 810` | Locks the memory clock at this MHz (RTX 30 and newer) |
| `-pl 250` | Power limit in watts. The card holds the nearest limit it allows |
| `-fan 75` | Fan speed in percent, every fan of the card |

One value sets every card. Cards that differ take a list, one value per card, in the order of
`-device`; `*` leaves that card as it is:

```
-device 0,1,2 -cclock 275,250,* -lock-cclock 2610,1350,1350 -fan 75
```

Here card 0 gets +275 and 2610 MHz, card 1 +250 and 1350 MHz, card 2 keeps its offset and gets
1350 MHz, and every fan runs at 75%. Without `-device`, the list follows the card numbers the
miner shows: 0, 1, 2…

- A list with the wrong number of values changes nothing on any card, and the log says so. If
  a card drops off the bus, the numbers shift; the miner then mines at stock instead of putting
  one card's clocks on another.
- Linux needs root ([with sudo](linux.md#with-sudo)) and Windows needs **Run as administrator**.
  HiveOS already runs the miner as root.
- On exit (Ctrl+C, or quitting the screen), the miner puts back what it changed: offsets 0,
  clocks unlocked, power limit back to the card's own, fans on auto. If it is killed, the settings stay until the next start.
- On HiveOS, use either these flags or the OC page for a card, not both: the miner's exit
  undoes Hive's lock and fan until Hive applies its profile again.

The panel's Control tab sets the same as HiveOS's OC page, for every card at once or card by
card, and keeps it per coin: the coin that mines uses its own when the miner starts. It is the
Manual mode of the Overclock panel: it runs with auto overclock off. While the miner runs in Manual
mode, changing the running coin's manual overclock applies to the cards at once without restarting
the miner. On Linux and Windows desktops, turning overclock on asks for root / Administrator
permissions automatically when the miner was started as a regular user. A coin with
nothing set there runs the flags above.

## Auto overclock

Supported for **Keryx** (every supported card) and **Proxima** (RTX 30 and RTX 50 cards); other
coins mine with the clocks the card has.

| Option | What it does |
| --- | --- |
| `-auto-oc` | The miner finds each card's core clock, undervolt, memory and fan, and keeps them |
| `-eco` | With `-auto-oc`, the default: the best hashrate per watt, cards at 70 °C at most |
| `-boost` | With `-auto-oc`: more hashrate while the extra power still pays, cards at 75 °C at most |
| `-retune` | With `-auto-oc`: tune every card again. `-retune 1,3` tunes cards 1 and 3 only |
| `-max-fan` | With `-auto-oc`: the most fan the miner may set, in percent. Default 100. `-max-fan 80,*,100` per card |

```
-auto-oc              eco on every card
-auto-oc -boost       boost on every card
-auto-oc -retune 2    eco, and tune card 2 again
-auto-oc -max-fan 80  eco, and never run the fan above 80%
```

- Every card is tuned at the same time; the whole rig takes about 30 to 45 minutes. The
  hashrate moves while it runs; the TUI shows `AUTO OC eco · tuning 4 of 4` (cards still
  tuning) and a `~` beside the hashrate of each card being measured. Leave the rig mining until
  the header says `tuned`.
- The result is saved per card, so the next start uses it at once. A new card or a new driver
  is tuned again by itself. `-retune` tunes a card again on demand.
- The miner sets the fan. With `-fan` the card keeps your fan and the tuning works under it.
  Above the temperature limit the fan goes up first, up to `-max-fan` (100% by default), and
  only at that limit does the clock come down. `-max-fan` cannot be combined with `-fan`.
  Changing `-max-fan` tunes the card again at the new fan, because the old temperatures were
  measured with other cooling.
- On cards that report their memory temperature (RTX 30 with GDDR6X, RTX 50, CMP 170HX and
  other HBM cards), the memory has its own limit, the same in `-eco` and `-boost`: 5 °C under
  the temperature where it slows itself down. GDDR6X slows down at 110 °C (memory at 105 °C at
  most), GDDR7 at 105 °C (100 °C); HBM cards use the limit their driver reports. When the memory runs over its limit, its clock comes down, not
  the core's. The log shows each card's memory limit at start. A card that starts reporting
  its memory temperature is tuned again once.
- A setting that crashed the miner or returned wrong results is never tried again. Tuning can
  restart the miner a few times; that is expected.
- A card given `-cclock`, `-mclock`, `-lock-cclock`, `-lock-mclock` or `-pl` is left to you and shows
  `manual`; the other cards are tuned.
- While a card answers an AI request it runs on the graphics driver's clocks, and the moment it
  mines again the overclock (yours or `-auto-oc`'s) comes back. Only the clocks the miner set
  are switched; a card with only `-fan`, or clocked by Hive, keeps what it has.
- This includes `-boost` and manual clocks: an aggressive overclock can crash token generation,
  so the driver's clocks are the safe baseline for every answer.
- Remove `-retune` once tuning is done: with it, every start tunes again.
- Same permissions as the flags above: root on Linux, **Run as administrator** on Windows.
- **HiveOS:** with `-auto-oc`, leave the rig's OC and autofan empty in Hive. Both would set the
  same clocks and fans, and the last one applied wins.

## Two coins on the same cards

Keryx can share each card with Proxima ([how to set it up](../../coins/keryx/README.md#proxima-on-the-same-cards)).
The first coin is the one `-coin` names; the second takes the `-dual-*` flags.

| Option | What it does | Default |
| --- | --- | --- |
| `-dual proxima` | Mine a second coin on the same cards at once | |
| `-dual-pool URL` | The second coin's pool, as `-pool` | required with `-dual` |
| `-dual-user NAME` | The second coin's pool username. `-worker` is added to it as to the wallet, or is the whole name when `-dual-user` is left out | |
| `-dual-password TOKEN` | The second coin's pool password or token | `x` |
| `-dual-keep 98` | The share of its hashrate, in percent, the first coin keeps; the second mines the rest of the card | `98` |
| `-dual-sms 16` | A fixed number of each card's SMs for the second coin, instead of measuring | |

## Environment variables

| Variable | What it does |
| --- | --- |
| `FCKHASH_DATA_DIR` | Folder for everything the miner keeps (models, escrow, settings, the log). Default `data` next to the miner; `/var/lib/fckhash` on HiveOS |
| `FCKHASH_SYNC=block` | Keryx: lower CPU use on rigs with many GPUs and few cores, for about 0.1% less hashrate |
| `FCKHASH_SYNC=spin` | Keryx: one full CPU core per GPU. Vela does not read `FCKHASH_SYNC` |

## Examples

| Goal | HiveOS Extra config | `start-<coin>.sh` / `start-<coin>.bat` |
| --- | --- | --- |
| Cards 0 and 1 only | `-device 0,1` | `-device 0,1` on the start line |
| Stats on the LAN | `-stats 0.0.0.0:4060` | `-stats 0.0.0.0:4060` on the start line |
| Data on another disk | | `FCKHASH_DATA_DIR=/data/fckhash` |
| Overclock, same on every card | `-cclock 275 -lock-cclock 2610 -lock-mclock 810 -fan 75` | the same, on the start line |
