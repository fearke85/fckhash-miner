# Keryx

**English** · [Português (Brasil)](README.pt-BR.md) · [All coins](../../README.md)

Mining Keryx with FCKHash. Algorithm: `keryxhash`.

> [!WARNING]
> **The H16 hard fork** activates at DAA 147,000,000 (2026-11-07, around 14:00 UTC). From then on
> versions before 1.6.0 are invalid: the network rejects their shares and blocks. Solo miners need
> keryx-node 1.6.7 or newer.

## Cards

- NVIDIA Turing or newer (RTX 20xx, 30xx, 40xx, 50xx) with 8 GB or more.
- NVIDIA CMP 170HX, including cards unlocked to 64 GB, which mine tier 4. Measured: ~3.2 MH/s per
  card at a 200 W power limit.
- Cards with 6 GB or less are not supported.
- Pascal (GTX 10xx, P104/P106) and older are not supported.

## Which tier does my card mine?

Each card mines the highest tier whose model fits in its memory with room left to answer that
tier's AI requests. You do not need `--tier`.

| Card memory | Tier | Free disk for the model |
| --- | --- | --- |
| 8-11 GB | 0 | ~6.7 GB |
| 12 GB | 1 | ~8.5 GB |
| 16-20 GB | 2 | ~10 GB |
| 24 GB | 3 | ~17 GB |
| 32 GB or more | 4 | ~30 GB |

Each AI answer is also kept on disk for 12 hours, tens of MB per answer, so leave a few GB free
beyond the model on a rig that answers many requests.

With a monitor attached, what the desktop takes does not count, and a card near the edge may
mine one tier lower.

## Settings

Install the package with your platform's guide, then use these values.

### HiveOS flight sheet

| Field | Value |
| --- | --- |
| Miner name | `fckhash` |
| Installation URL | `https://github.com/fearke85/fckhash-miner/releases/download/v1.0.2/fckhash-1.0.2.tar.gz` |
| Hash algorithm | `keryxhash` |
| Wallet and worker template | Pool: `%WAL%.%WORKER_NAME%`. Solo: `%WAL%` |
| Pool URL | Pool: `stratum+tcp://krx.suprnova.cc:4404`, or `stratum+ssl://krx.cryo-pool.com:5555`. Solo: `grpc://IP:PORT` of your node |
| Password | `x`, or the pool password |

The wallet is your Keryx address, `keryx:...`. Install guide: [HiveOS](../../docs/en/hiveos.md).

### start-keryx.sh (Linux) and start-keryx.bat (Windows)

The pool line is already filled in. Put your address in `-wallet`. For solo, comment the pool
line and uncomment the solo line.

| What | Value |
| --- | --- |
| `-wallet` | Your Keryx address, `keryx:...` |
| `-worker` | A name for this rig on the pool |
| `-pool` | `krx.suprnova.cc:4404` |
| `-node` | Solo only: `grpc://IP:PORT` of your node |
| `--escrow-cert` | Solo only, first start: see [solo mining](docs/en/solo.md) |

### Pools

| Pool | Address | Transport |
| --- | --- | --- |
| Suprnova | `krx.suprnova.cc:4404` (EU), `stratum-us.suprnova.cc:4404` (US) | plain; `stratum+ssl://krx.suprnova.cc:4405` also works |
| Cryo Pool | `krx.cryo-pool.com:5555` | encrypted only: `stratum+ssl://krx.cryo-pool.com:5555` |

`-pool` takes a bare `host:port` or a stratum URL; `stratum+ssl://` (or `stratum+tls://`) wraps
the connection in TLS. The miner checks the pool's certificate, so an address without a valid
certificate does not connect. The same wallet and worker work on both pools; each one has its own
dashboard. On HiveOS, put the full URL in the Pool URL field.

Install guides: [Linux](../../docs/en/linux.md) · [Windows](../../docs/en/windows.md).

## Proxima on the same cards

A card can mine Proxima beside Keryx at once: Keryx keeps nearly all of its hashrate (98% by
default) and Proxima mines the room it leaves. Add four flags to the Keryx settings; nothing else
changes. Linux, HiveOS and Windows (not WSL). The miner measures each card at the first start
and shares it only when the two coins do more work together than one alone; a card where no split
keeps 98% of Keryx takes the split that adds the most work (`-dual-keep 100` leaves it to Keryx).
In a rig with other cards, those mine Keryx alone.

| Flag | Value |
| --- | --- |
| `-dual` | `proxima` |
| `-dual-pool` | `stratum+tcp://us3.innovlab.cc:19701` (`eu3.innovlab.cc:19701` in Europe), `http://prox.ariabrain.com:3359` (AriaPool) or `stratum+ssl://proxi.suprnova.cc:3356` (Suprnova) |
| `-dual-user` | InnovLab: your Proxima address. AriaPool: a name for this rig. Suprnova: `ACCOUNT.WORKER` |
| `-dual-password` | InnovLab / Suprnova: `x`. AriaPool: your account token |

On HiveOS keep the flight sheet as above and put the flags in **Extra config arguments** (or enable
**Dual mining** in the [Web Panel's Control tab](../../docs/en/panel.md)):

```
-dual proxima -dual-pool stratum+tcp://us3.innovlab.cc:19701 -dual-user YOUR_PROXIMA_ADDRESS.%WORKER_NAME%
```

With `start-keryx.sh`, add them to the pool line. There `-worker` names the
rig on both pools: give only your Proxima address on InnovLab, leave `-dual-user` out on AriaPool,
give only `ACCOUNT` on Suprnova. Standalone Proxima guide: [coins/proxima](../proxima/README.md).

- **First start:** each card measures how the two coins share it, about 4 to 8 minutes, mining
  both the whole time. The result is kept and measured again only after an update or a new
  overclock.
- **AI requests:** Proxima stops for the length of each answer, and Keryx answers on the whole
  card as before.
- **Screen and stats:** Proxima's hashrate and shares on the miner's screen, beside Keryx's and
  per card; `"dual"` in `/stats`; Hive's second algorithm.
- **Overclock:** `-auto-oc` keeps tuning the cards that mine Keryx alone. A card that mines two
  coins keeps the clocks you give it with `-lock-cclock` and `-cclock`, or the driver's. Two
  coins draw more power and heat than Keryx alone.
- **How much Keryx keeps:** `-dual-keep 95` gives Proxima more of the card for up to 5% of
  Keryx's hashrate. Options: [miner options](../../docs/en/options.md#two-coins-on-the-same-cards).

## AI requests

Keryx sends AI requests to the miners of each tier, and every miner of that tier has to answer
them in time. The miner answers them by itself while it mines. A request your rig does not answer
counts as a strike against your payout address: keep the miner running and the cards stable, and
check `log.txt`. More in [troubleshooting](docs/en/troubleshooting.md).

Since H15 the network gives far less time to answer. A request that arrives while the model is
not loaded may be answered late.

The miner's screen shows **AI processed**, the answers your pool or node accepted, and in solo
**AI paid**, the AI rewards the network paid to your wallet and their KRX. A reward arrives about
12 hours after its answer; one that arrives while the miner is stopped is paid but not counted.
Both counts are kept per wallet across restarts. Rigs that mine to the same wallet each count all
of that wallet's rewards.

## More

- [Solo mining](docs/en/solo.md)
- [Keryx options](docs/en/options.md) (after `--`)
- [Keryx troubleshooting](docs/en/troubleshooting.md)
- [Miner options and general troubleshooting](../../docs/en/options.md)
