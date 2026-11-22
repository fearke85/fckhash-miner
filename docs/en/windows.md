# Windows

**English** · [Português (Brasil)](../pt-BR/windows.md) · [Back to README](../../README.md)

How to install FCKHash on Windows, for any coin. Visual guide to the built-in web panel
(`http://127.0.0.1:4455` or `http://RIG-IP:4455`): [Web Panel & Multi-Rig Hub](panel.md). The values
for your coin are on its page:
[Keryx](../../coins/keryx/README.md#start-keryxsh-linux-and-start-keryxbat-windows), [Vela](../../coins/vela/README.md#start-velash-linux-and-start-velabat-windows), [Proxima](../../coins/proxima/README.md#start-proximash-linux-and-start-proximabat-windows), [NEB](../../coins/neb/README.md#start-nebsh-linux-and-start-nebbat-windows), [Numen](../../coins/numen/README.md#start-numensh-linux-and-start-numenbat-windows), [Vearl](../../coins/vearl/README.md#start-vearlsh-linux-and-start-vearlbat-windows).

## Before you start

- Windows 10 or 11, 64-bit.
- NVIDIA driver 580 or newer, from nvidia.com or the NVIDIA app. The package brings the CUDA
  libraries.
- A card your coin supports, and the free disk your coin's page asks for, next to the miner.

## Install

1. Download `fckhash-windows-1.0.2.zip` from the
   [release page](https://github.com/fearke85/fckhash-miner/releases/tag/v1.0.2).
2. Extract it to a folder of its own, for example `C:\fckhash`.
3. Right-click `start-keryx.bat` (Vela: `start-vela.bat`), choose **Edit** (or open it in Notepad).

## Settings in the start script

Each coin has its own script: `start-keryx.bat` for Keryx, `start-vela.bat` for Vela, `start-numen.bat` for Numen. Leave one
line uncommented, the pool line or the solo line, and put your address in `-wallet`. The pool, the node and the other
values are on your coin's page.

Other options go on that same line. Miner options: [options.md](options.md).

## Run

Save the script and double-click it. Keep the window open: closing it stops the miner.
When CUDA dies on a card, the miner exits and the script starts it again after 3 seconds.

### The panel in the Start Menu

The first start adds **FCKHash Panel** to the Start Menu, with the FCKHash icon. It opens the panel
in the default browser; the miner does not have to be running, and the browser then shows that the
address did not answer.

To put the entry back after moving the folder or changing `-panel`, run in a Command Prompt:

```
fckhash.exe -shortcut
```

To take it out, delete **FCKHash Panel** from the Start Menu.

For a window without the browser bars, install the panel as an app: open it in Edge or Chrome and
press **Install**. That is the entry to pin to the taskbar.

## Dual mining

A card can mine Proxima beside Keryx on Windows: the miner measures each card at the first start
and shares it only when the two coins do more work together than one alone. Add the `-dual*` flags
to `start-keryx.bat`; the [Keryx page](../coins/keryx/README.md#proxima-on-the-same-cards) has the
pool values. A card that shares keeps the clocks and the fan you set with `-lock-cclock`,
`-cclock` and `-fan`; auto OC leaves it alone. `-dual-keep 100` keeps a card on Keryx alone.

## WSL

The Linux miner runs in WSL 2, but the WSL CUDA stack is not supported for mining: on our rig
(WSL 3.0.5.0, NVIDIA driver 617.42 and 621.24) it fails to start CUDA, so the miner says so and
mines one coin. Update WSL (`wsl --update`) and reinstall the latest NVIDIA Windows driver with
the **WSL** component, then reboot; the App/Windows Update path can leave the WSL libraries at an
older version. The clocks, fans and overclock are set on Windows, not inside WSL. For two coins
use the native Windows miner above.

## Update to a new version

Install new releases directly from the [Web Panel](panel.md) (press **Install** in the top bar, turn
on **Auto-update**, or press **Install on every rig** from a hub panel): your start script, settings,
history and `data` stay untouched, and the miner rolls back automatically if a new version does not
mine.

To update by hand, extract the new zip over the old folder and choose **Skip** when asked to replace
your edited `start-*.bat`. What the miner keeps is in `data` and stays.

From fear-miner: rename the `fear-miner` folder to `fckhash`, then extract over it. The first
start moves what the miner keeps (`keryx`, `vela`, the log) into `data` by itself; the models are
not downloaded again. What it no longer uses goes into `data\old`: delete it once the miner mines
as before.

## Where things are

| What | Where |
| --- | --- |
| Log with warnings, errors and crash reports | `data\logs\log.txt` next to `fckhash.exe` |
| Data the miner keeps | `data`, a folder per coin. Back up `data\keryx\escrow`. Set `FCKHASH_DATA_DIR` to move it |
| Hashrate and shares | `http://127.0.0.1:4060/stats` in a browser |
