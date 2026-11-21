# Changelog

What changed in each version of FCKHash, newest first. One version covers every coin; a change
for one coin names it.

FCKHash 1.0.0 follows fear-miner 1.6.8: the same miner under a new name, with its own version
numbers. What changed up to fear-miner 1.6.8 is in the
[fear-miner changelog](https://github.com/fearke85/fear-miner/blob/main/CHANGELOG.md).

## 1.0.0 (2026-11-21)

First release under the **FCKHash** name, following fear-miner 1.6.8. New flags: `-panel`, `-open`,
`-shortcut`, `-update`, `-pl`, `-dual`, `-dual-pool`, `-dual-user`, `-dual-password`, `-dual-keep`,
`-dual-sms`. New coins: `proxima`, `neb`.

- **fear-miner is now FCKHash:** mining and settings stay the same.
  - **HiveOS:** set the flight sheet's new Installation URL (`fckhash-1.0.0.tar.gz`) and the miner
    name `fckhash`; `/var/lib/fear-miner` moves to `/var/lib/fckhash` by itself.
  - **Linux and Windows:** rename your `fear-miner` folder to `fckhash`, then unpack the new
    archive over it.
  - **Environment variables:** `FEAR_DATA_DIR` and `FEAR_SYNC` still work; rename them to
    `FCKHASH_DATA_DIR` and `FCKHASH_SYNC`.
  - **One clean data folder:** the first start reorganises the data folder by itself (on Linux and
    Windows everything moves into `data/` beside the miner; models are not downloaded again) and
    keeps what it no longer uses in `old/`, which is safe to delete once the rig mines as before.
    The Keryx escrow backup path is now `data/keryx/escrow` on Linux and Windows, and
    `/var/lib/fckhash/keryx/escrow` on HiveOS.
- **Built-in Web Panel & Multi-Rig Hub ([panel guide](docs/en/panel.md)):** every rig serves a web
  panel on port `4455` (`-panel ADDRESS:PORT`, or `-panel off`), in English and Portuguese, with no
  extra program to install.
  - **5-step Setup Wizard & Control tab:** pick the coin, pool or node, wallet, worker, mining GPUs,
    coin options and dual mining from the browser. Settings saved in the panel survive miner updates
    and run over the start line until you edit the flight sheet or start file.
  - **Multi-Rig Hub & LAN auto-discovery:** scan your local network and Tailscale with one click to
    add your other rigs. The hub reads them every 5 seconds, keeps up to 400 days of their history
    (charts from 1 hour to 30 days), and lets you inspect and configure every rig from one page.
  - **Live manual overclock & Auto OC in the panel:** set core and memory offsets and locks, power
    limit (`-pl`, in watts) and fan speed for all cards or card by card, saved per coin. Changing
    the running coin's manual overclock applies to the cards at once without restarting the miner.
    On Linux and Windows desktops, turning overclock on asks for root or Administrator permission
    automatically.
  - **Keryx live AI view:** dedicated tab showing AI requests answered and paid, cards in
    quarantine, escrow status, each card's mining tier and loaded AI model, and the last 30 AI
    requests with tokens and time taken.
  - **Phone app (Tailscale HTTPS) & desktop app menu:** connect Tailscale and create an HTTPS
    address from the panel to install FCKHash Panel as a full-screen app on your phone. The first
    desktop start also adds **FCKHash Panel** to the Linux app menu and Windows Start Menu
    (`-open`, `-shortcut`).
  - **Start / Stop mining, muted alerts & 1-click log export:** stop and start mining from the top
    bar while keeping the panel online, mute known alerts across all devices, filter 30 days of logs
    by rig, level and coin, and export a single diagnostic `.zip` (with passwords and tokens
    removed) for support.
- **One-click & automatic updates (`-update manual|auto|off`):** every 3 hours the miner checks for
  a new signed release. On Linux and Windows, press **Install** in the panel, press **Install on
  every rig** from the hub to update the whole farm at once, or turn on **Auto-update** (`-update
  auto`): the miner verifies the signed package, waits until no AI request is running, keeps your
  start file and `data/` untouched, and rolls back to the previous version automatically if the new
  one does not mine. Before every start, the miner also checks its program files against the
  release's signed list and restarts itself automatically after an unexpected crash on Linux,
  Windows and HiveOS.
- **New coin — Proxima (`-coin proxima` and `-dual proxima`, [guide](coins/proxima/README.md)):**
  - **Dual mining with Keryx on RTX 50 cards (Linux and HiveOS):** mine Proxima on the same cards at
    the same time while Keryx keeps 98% of its hashrate by default (`-dual-keep 98`). Proxima
    pauses automatically while a card answers a Keryx AI request.
  - **Standalone pool and solo mining (RTX 20 and newer):** mine on **InnovLab** (no token needed,
    login with your Proxima address), **AriaPool**, **Suprnova SSL**, or solo against your own
    `proxi node mine` listener.
  - **Auto overclock for Proxima (`-auto-oc`):** supported on RTX 30 and RTX 50 cards, locking
    memory at its lowest power state (`405 MHz`, saving 14 to 47 W per card on RTX 30) and tuning
    the core lock and undervolt.
- **New coin — NEB (`-coin neb`, [guide](coins/neb/README.md)):** pool mining on **AriaPool**
  (Europe, Asia and solo port) and **Suprnova** (Europe, US, Asia and SSL), plus solo mining on your
  own NEB node, across RTX 20, 30, 40 and 50 cards.
- **Memory temperature protection in Auto OC:** on cards that report memory temperature (RTX 30 with
  GDDR6X, RTX 50 with GDDR7, CMP 170HX and HBM cards, on Linux and Windows), `-auto-oc` caps memory
  5 °C below its slowdown limit (105 °C on GDDR6X, 100 °C on GDDR7) and steps the memory clock down
  when needed.
