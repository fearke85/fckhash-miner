# Linux

**English** · [Português (Brasil)](../pt-BR/linux.md) · [Back to README](../../README.md)

How to install FCKHash on a plain Linux machine, outside HiveOS, for any coin. Visual guide to the
built-in web panel (`http://RIG-IP:4455`): [Web Panel & Multi-Rig Hub](panel.md). The values for
your coin are on its page: [Keryx](../../coins/keryx/README.md#start-keryxsh-linux-and-start-keryxbat-windows), [Vela](../../coins/vela/README.md#start-velash-linux-and-start-velabat-windows), [Proxima](../../coins/proxima/README.md#start-proximash-linux-and-start-proximabat-windows), [NEB](../../coins/neb/README.md#start-nebsh-linux-and-start-nebbat-windows), [Numen](../../coins/numen/README.md#start-numensh-linux-and-start-numenbat-windows), [Vearl](../../coins/vearl/README.md#start-vearlsh-linux-and-start-vearlbat-windows).

## Before you start

- x86-64 Linux with glibc 2.35 or newer (Ubuntu 22.04, Debian 12 and newer).
- NVIDIA driver 580 or newer. Check with `nvidia-smi`. The package brings the CUDA libraries.
- A card your coin supports, and the free disk your coin's page asks for, next to the miner.

## Install

```
wget https://github.com/fearke85/fckhash-miner/releases/download/v1.0.0/fckhash-linux-1.0.0.tar.gz
tar -xzf fckhash-linux-1.0.0.tar.gz
cd fckhash
nano start-keryx.sh        # Vela: start-vela.sh
```

## Settings in the start script

Each coin has its own script: `start-keryx.sh` for Keryx, `start-vela.sh` for Vela, `start-numen.sh` for Numen. Leave one line
uncommented, the pool line or the solo line, and put your address in `-wallet`. The pool, the node and the other
values are on your coin's page.

Other options go on that same line. Miner options: [options.md](options.md).

## Run

```
./start-keryx.sh           # Vela: ./start-vela.sh
```

To keep the miner running after you log out, start it in `screen` or `tmux`, or as a systemd
service.

### The panel in the app menu

The first start with a desktop adds **FCKHash Panel** to the app menu, with the FCKHash icon. It
opens the panel in a window of its own when Chrome, Edge, Brave or Chromium is installed, else in
the default browser. The miner does not have to be running: the window then says the rig does not
answer.

To put the entry back after moving the folder or changing `-panel`, run:

```
./fckhash -shortcut
```

To take it out, delete `~/.local/share/applications/fckhash-panel.desktop`.

The panel also installs as an app from Chrome and Edge: open it and press **Install**. That is the
window without the browser bars. Firefox does not install web apps, so on it the app menu entry is
the way.

### With sudo

The overclock flags (`-cclock`, `-mclock`, `-lock-cclock`, `-lock-mclock`, `-fan`) need root:

```
sudo ./start-keryx.sh      # Vela: sudo ./start-vela.sh
```

Started with `sudo` or `su` from your own login, the miner creates its files (the `data`
folder) as the owner of the miner folder, so you can still edit and delete them. It also gives
back what an earlier root run left there. A systemd service or a root login keeps root's files; to
take them back, run `sudo chown -R "$USER": .` in the miner folder.

## Update to a new version

Install new releases directly from the [Web Panel](panel.md) (press **Install** in the top bar, turn
on **Auto-update**, or press **Install on every rig** from a hub panel): your start script, settings,
history and `./data` stay untouched, and the miner rolls back automatically if a new version does
not mine.

To update by hand, unpack the new archive over the old folder, keeping your edited start script.
What the miner keeps is in `./data` and stays.

From fear-miner: rename the `fear-miner` folder to `fckhash`, then unpack over it. The first
start moves what the miner keeps (`keryx`, `vela`, the log) into `./data` by itself; the models
are not downloaded again. What it no longer uses goes into `./data/old`: delete it once the miner
mines as before.

## Where things are

| What | Where |
| --- | --- |
| Log with warnings, errors and crash reports | `./data/logs/log.txt` |
| Data the miner keeps | `./data`, a folder per coin. Back up `./data/keryx/escrow`. Set `FCKHASH_DATA_DIR` to move it |
| Hashrate and shares | `curl 127.0.0.1:4060/stats` |
