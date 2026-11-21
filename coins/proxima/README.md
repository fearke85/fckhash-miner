# Proxima

**English** · [Português (Brasil)](README.pt-BR.md) · [All coins](../../README.md)

Mining Proxima (PROX) with FCKHash. Algorithm: `proxima` (`prox` on HiveOS). Pool mining on
**InnovLab**, **AriaPool** or **Suprnova**, solo mining as a seeker on your own `proxi node mine`
listener, or **dual mining on the same cards as Keryx** (RTX 50).

The miner shows its speed in **GH/s** and its efficiency in **MH/J**.

## Cards

- NVIDIA Turing or newer (RTX 20xx, 30xx, 40xx, 50xx).
- Pascal (GTX 10xx) and older are not supported.
- **Auto overclock (`-auto-oc`):** supported on RTX 30 and RTX 50 cards when mining Proxima
  standalone. Proxima's speed follows the core clock alone, so auto overclock locks the memory at
  its lowest power state (`405 MHz`, saving 14 to 47 W per card on RTX 30) and tunes the core lock
  and undervolt for the best efficiency (`-eco`, default) or speed (`-boost`).

## Settings

Install the package with your platform's guide, then configure from the [Web Panel](../../docs/en/panel.md)
(`http://RIG-IP:4455`) or use the values below.

### Pools

| Pool | `-pool` on Linux and Windows | Pool URL on HiveOS | Login (`-wallet` / `-user`) | Password (`-password`) |
| --- | --- | --- | --- | --- |
| **InnovLab (US)** | `stratum+tcp://us3.innovlab.cc:19701` | `stratum+tcp://us3.innovlab.cc:19701` | Your Proxima address (`.WORKER`) | `x` (no token needed) |
| **InnovLab (EU)** | `stratum+tcp://eu3.innovlab.cc:19701` | `stratum+tcp://eu3.innovlab.cc:19701` | Your Proxima address (`.WORKER`) | `x` (no token needed) |
| **AriaPool** | `http://prox.ariabrain.com:3359` | `http://prox.ariabrain.com:3359` | Worker name (`rig1`) | Your AriaPool account token |
| **AriaPool (Solo)** | `http://prox.ariabrain.com:3369` | `http://prox.ariabrain.com:3369` | Worker name (`rig1`) | Your AriaPool account token |
| **Suprnova (SSL)** | `stratum+ssl://proxi.suprnova.cc:3356` | `stratum+ssl://proxi.suprnova.cc:3356` | `ACCOUNT.WORKER` | `x`, or the worker password |

### HiveOS flight sheet

| Field | Value |
| --- | --- |
| Miner name | `fckhash` |
| Installation URL | `https://github.com/fearke85/fckhash-miner/releases/download/v1.0.1/fckhash-1.0.1.tar.gz` |
| Hash algorithm | `proxima` (or put `-coin proxima` in Extra config arguments) |
| Wallet and worker template | InnovLab / Suprnova: `%WAL%.%WORKER_NAME%`. AriaPool: `%WORKER_NAME%` |
| Pool URL | `stratum+tcp://us3.innovlab.cc:19701` (or any pool from the table above) |
| Password | InnovLab / Suprnova: `x`. AriaPool: your account token |
| Extra config arguments | `-coin proxima` (add `-auto-oc` on RTX 30 / RTX 50 rigs) |

Install guide: [HiveOS](../../docs/en/hiveos.md).

### start-proxima.sh (Linux) and start-proxima.bat (Windows)

The InnovLab pool line is ready out of the box. Put your Proxima payout address in `-wallet`:

| What | Value |
| --- | --- |
| `-wallet` | Your Proxima address (on Suprnova, your Suprnova account; on AriaPool, omit and use `-worker`) |
| `-worker` | A name for this rig on the pool |
| `-pool` | `stratum+tcp://us3.innovlab.cc:19701` |
| `-password` | `x` on InnovLab and Suprnova; your account token on AriaPool |

Install guides: [Linux](../../docs/en/linux.md) · [Windows](../../docs/en/windows.md).

### Dual mining beside Keryx (RTX 50)

RTX 50 cards on Linux and HiveOS can mine Proxima on the same cards while mining Keryx: Keryx keeps
98% of its hashrate by default and Proxima uses the spare room. Turn on **Dual mining** in the
[Web Panel's Control tab](../../docs/en/panel.md#control-live-manual-overclock--auto-oc) or follow
the [Keryx dual mining setup](../keryx/README.md#proxima-on-the-same-cards).

### Solo with your own Proxima node (`proxi node mine`)

Run `proxi node mine --workers 0` with its seeker listener enabled (`localhost:18555` and a seeker
token in your node profile), then point FCKHash at it:

```
./fckhash -coin proxima -node localhost:18555 -password YOUR_SEEKER_TOKEN
```

## Proxima options

Coin options take two hyphens. Normally you do not need them because `-user` / `-worker` and
`-password` already set the seeker name and token:

| Option | What it does |
| --- | --- |
| `--token TOKEN` | Bearer token for AriaPool or `proxi node mine` (same as `-password TOKEN`) |
| `--name NAME` | Seeker or worker name shown by the node or pool (same as `-user` / `-worker`) |

## Help

- [Discord](https://discord.gg/uK93NPAhbd)
- [Open an issue](https://github.com/fearke85/fckhash-miner/issues/new/choose) and attach the panel's log export or `log.txt`
