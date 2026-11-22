# NEB

**English** · [Português (Brasil)](README.pt-BR.md) · [All coins](../../README.md)

Mining NEB with FCKHash. Algorithm: `neb`. Pool mining on **AriaPool** or **Suprnova**, solo mining
through AriaPool's solo port, and solo mining on your own NEB node.

The miner shows its speed in **H/s** (`kH/s` or `MH/s`).

## Cards

- NVIDIA Turing or newer (RTX 20xx, 30xx, 40xx, 50xx, CMP 40HX/50HX/170HX).
- Pascal (GTX 10xx) and older are not supported.

## Dataset file (`neb-datasets.bin`)

NEB mining uses the network's dataset file, `neb-datasets.bin` (16 MB). The miner downloads it
automatically from the official NEB node release on first start and keeps it in the data folder, or
you can pass `--dataset PATH`:

| Platform | Default location |
| --- | --- |
| HiveOS | `/var/lib/fckhash/neb/neb-datasets.bin` |
| Linux | `data/neb/neb-datasets.bin` (or next to `fckhash`) |
| Windows | `data\neb\neb-datasets.bin` (or next to `fckhash.exe`) |

## Settings

Install the package with your platform's guide, then configure from the [Web Panel](../../docs/en/panel.md)
(`http://RIG-IP:4455`) or use the values below.

### Pools

| Pool | Mode | `-pool` on Linux and Windows | Pool URL on HiveOS |
| --- | --- | --- | --- |
| **AriaPool (Europe)** | shared (PPLNS) | `neb.ariabrain.com:3370` | `neb.ariabrain.com:3370` |
| **AriaPool (Asia)** | shared (PPLNS) | `hk.neb.ariabrain.com:3370` | `hk.neb.ariabrain.com:3370` |
| **AriaPool Solo** | solo | `neb.ariabrain.com:3371` | `neb.ariabrain.com:3371` |
| **Suprnova (Europe)** | shared | `stratum+tcp://neb.suprnova.cc:7804` | `stratum+tcp://neb.suprnova.cc:7804` |
| **Suprnova (US)** | shared | `stratum+tcp://stratum-us.suprnova.cc:7804` | `stratum+tcp://stratum-us.suprnova.cc:7804` |
| **Suprnova (Asia)** | shared | `stratum+tcp://stratum-apac.suprnova.cc:7804` | `stratum+tcp://stratum-apac.suprnova.cc:7804` |
| **Suprnova (SSL)** | shared (encrypted) | `stratum+ssl://neb.suprnova.cc:7806` | `stratum+ssl://neb.suprnova.cc:7806` |

The wallet is your NEB bech32m payout address (`neb1...`).

### HiveOS flight sheet

| Field | Value |
| --- | --- |
| Miner name | `fckhash` |
| Installation URL | `https://github.com/fearke85/fckhash-miner/releases/download/v1.0.2/fckhash-1.0.2.tar.gz` |
| Hash algorithm | `neb` (or put `-coin neb` in Extra config arguments) |
| Wallet and worker template | `%WAL%.%WORKER_NAME%` |
| Pool URL | `neb.ariabrain.com:3370` or `stratum+tcp://neb.suprnova.cc:7804` |
| Password | `x` |
| Extra config arguments | `-coin neb` |

Install guide: [HiveOS](../../docs/en/hiveos.md).

### start-neb.sh (Linux) and start-neb.bat (Windows)

Put your `neb1...` address in `-wallet`:

| What | Value |
| --- | --- |
| `-wallet` | Your NEB address (`neb1...`) |
| `-worker` | A name for this rig on the pool: 1 to 32 letters, digits, `_` or `-` |
| `-pool` | `neb.ariabrain.com:3370` (or Suprnova `stratum+tcp://neb.suprnova.cc:7804`) |

Install guides: [Linux](../../docs/en/linux.md) · [Windows](../../docs/en/windows.md).

### Solo on your own NEB node

Point `-node` at your NEB node's work service port (default `3441`). The node pays blocks to the
mining wallet configured on the node itself:

```
./fckhash -coin neb -node localhost:3441
```

## NEB options

Coin options take two hyphens. On HiveOS they go in Extra config arguments:

| Option | What it does |
| --- | --- |
| `--dataset PATH` | Path to `neb-datasets.bin` when it is not in `data/neb/` or next to the miner |

## Troubleshooting

| What you see | What to do |
| --- | --- |
| `neb-datasets.bin was not found` | Copy `neb-datasets.bin` from the NEB node release into `data/neb/` (`/var/lib/fckhash/neb/` on HiveOS), or pass `--dataset /path/to/neb-datasets.bin`. |
| `payout address prefix ... (expected neb1…)` | Use a valid mainnet `neb1...` bech32m address from `neb-wallet address`. |

General problems: [troubleshooting](../../docs/en/troubleshooting.md).

## Help

- [Discord](https://discord.gg/uK93NPAhbd)
- [Open an issue](https://github.com/fearke85/fckhash-miner/issues/new/choose) and attach the panel's log export or `log.txt`
