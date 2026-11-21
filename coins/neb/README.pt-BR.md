# NEB

[English](README.md) · **Português (Brasil)** · [Todas as moedas](../../README.pt-BR.md)

Mineração de NEB com o FCKHash. Algoritmo: `neb`. Mineração em pool na **AriaPool** ou
**Suprnova**, solo pela porta solo da AriaPool, e solo no seu próprio node NEB.

O miner mostra a velocidade em **H/s** (`kH/s` ou `MH/s`).

## Placas

- NVIDIA Turing ou mais nova (RTX 20xx, 30xx, 40xx, 50xx, CMP 40HX/50HX/170HX).
- Pascal (GTX 10xx) e anteriores não são suportadas.

## Arquivo de dataset (`neb-datasets.bin`)

A mineração de NEB usa o arquivo de dataset da rede, `neb-datasets.bin` (16 MB). O miner baixa o
arquivo automaticamente da release oficial do node NEB na primeira execução e o guarda na pasta de
dados, ou você pode passar `--dataset CAMINHO`:

| Plataforma | Local padrão |
| --- | --- |
| HiveOS | `/var/lib/fckhash/neb/neb-datasets.bin` |
| Linux | `data/neb/neb-datasets.bin` (ou ao lado do `fckhash`) |
| Windows | `data\neb\neb-datasets.bin` (ou ao lado do `fckhash.exe`) |

## Configuração

Instale o pacote seguindo o guia da sua plataforma e configure pelo
[Painel Web](../../docs/pt-BR/panel.md) (`http://IP-DA-RIG:4455`) ou use os valores abaixo.

### Pools

| Pool | Modo | `-pool` no Linux e Windows | Pool URL no HiveOS |
| --- | --- | --- | --- |
| **AriaPool (Europa)** | compartilhado (PPLNS) | `neb.ariabrain.com:3370` | `neb.ariabrain.com:3370` |
| **AriaPool (Ásia)** | compartilhado (PPLNS) | `hk.neb.ariabrain.com:3370` | `hk.neb.ariabrain.com:3370` |
| **AriaPool Solo** | solo | `neb.ariabrain.com:3371` | `neb.ariabrain.com:3371` |
| **Suprnova (Europa)** | compartilhado | `stratum+tcp://neb.suprnova.cc:7804` | `stratum+tcp://neb.suprnova.cc:7804` |
| **Suprnova (EUA)** | compartilhado | `stratum+tcp://stratum-us.suprnova.cc:7804` | `stratum+tcp://stratum-us.suprnova.cc:7804` |
| **Suprnova (Ásia)** | compartilhado | `stratum+tcp://stratum-apac.suprnova.cc:7804` | `stratum+tcp://stratum-apac.suprnova.cc:7804` |
| **Suprnova (SSL)** | compartilhado (criptografado) | `stratum+ssl://neb.suprnova.cc:7806` | `stratum+ssl://neb.suprnova.cc:7806` |

A carteira é o seu endereço de pagamento NEB no formato bech32m (`neb1...`).

### Flight sheet no HiveOS

| Campo | Valor |
| --- | --- |
| Miner name | `fckhash` |
| Installation URL | `https://github.com/fearke85/fckhash-miner/releases/download/v1.0.1/fckhash-1.0.1.tar.gz` |
| Hash algorithm | `neb` (ou coloque `-coin neb` em Extra config arguments) |
| Wallet and worker template | `%WAL%.%WORKER_NAME%` |
| Pool URL | `neb.ariabrain.com:3370` ou `stratum+tcp://neb.suprnova.cc:7804` |
| Password | `x` |
| Extra config arguments | `-coin neb` |

Guia de instalação: [HiveOS](../../docs/pt-BR/hiveos.md).

### start-neb.sh (Linux) e start-neb.bat (Windows)

Coloque o seu endereço `neb1...` em `-wallet`:

| O quê | Valor |
| --- | --- |
| `-wallet` | Seu endereço NEB (`neb1...`) |
| `-worker` | Um nome para esta rig na pool: 1 a 32 letras, dígitos, `_` ou `-` |
| `-pool` | `neb.ariabrain.com:3370` (ou Suprnova `stratum+tcp://neb.suprnova.cc:7804`) |

Guias de instalação: [Linux](../../docs/pt-BR/linux.md) · [Windows](../../docs/pt-BR/windows.md).

### Solo no seu próprio node NEB

Aponte o `-node` para a porta do serviço de trabalho do seu node NEB (padrão `3441`). O node paga
os blocos diretamente para a carteira de mineração configurada no próprio node:

```
./fckhash -coin neb -node localhost:3441
```

## Opções da NEB

Opções da moeda levam dois hífens. No HiveOS elas vão em Extra config arguments:

| Opção | O que faz |
| --- | --- |
| `--dataset CAMINHO` | Caminho para o `neb-datasets.bin` quando ele não estiver em `data/neb/` ou ao lado do miner |

## Problemas

| O que aparece | O que fazer |
| --- | --- |
| `neb-datasets.bin was not found` | Copie o `neb-datasets.bin` da release do node NEB para `data/neb/` (`/var/lib/fckhash/neb/` no HiveOS) ou passe `--dataset /caminho/para/neb-datasets.bin`. |
| `payout address prefix ... (expected neb1…)` | Use um endereço `neb1...` válido da mainnet gerado por `neb-wallet address`. |

Problemas gerais: [solução de problemas](../../docs/pt-BR/troubleshooting.md).

## Ajuda

- [Discord](https://discord.gg/uK93NPAhbd)
- [Abra uma issue](https://github.com/fearke85/fckhash-miner/issues/new/choose) e anexe a exportação de logs do painel ou o `log.txt`
