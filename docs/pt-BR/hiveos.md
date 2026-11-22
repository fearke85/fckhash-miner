# HiveOS

[English](../en/hiveos.md) · **Português (Brasil)** · [Voltar ao README](../../README.pt-BR.md)

Como instalar o FCKHash no HiveOS, para qualquer moeda. Guia visual do painel web integrado
(`http://IP-DA-RIG:4455`): [Painel Web e Hub Multi-Rig](panel.md). Os valores da sua moeda
(algoritmo, template da carteira, Pool URL) estão na página dela: [Keryx](../../coins/keryx/README.pt-BR.md#flight-sheet-do-hiveos),
[Vela](../../coins/vela/README.pt-BR.md#flight-sheet-na-hiveos),
[Proxima](../../coins/proxima/README.pt-BR.md#flight-sheet-no-hiveos),
[NEB](../../coins/neb/README.pt-BR.md#flight-sheet-no-hiveos),
[Numen](../../coins/numen/README.pt-BR.md#flight-sheet-na-hiveos),
[Vearl](../../coins/vearl/README.pt-BR.md#flight-sheet-na-hiveos).

## Antes de começar

- HiveOS com a imagem Ubuntu 22.04 (jammy). A imagem 20.04 não é suportada.
- Driver NVIDIA 580 ou mais novo. Atualize pelo painel do HiveOS ou com `nvidia-driver-update`.
- Uma placa que a sua moeda suporte, e o espaço em disco que a página da moeda pede.

## Flight sheet

Crie um flight sheet com o miner **Custom**:

| Campo | Valor |
| --- | --- |
| Miner name | `fckhash` |
| Installation URL | `https://github.com/fearke85/fckhash-miner/releases/download/v1.0.2/fckhash-1.0.2.tar.gz` |
| Hash algorithm | da página da sua moeda |
| Wallet and worker template | da página da sua moeda |
| Pool URL | da página da sua moeda |
| Password | `x`, ou a senha da pool |
| Extra config arguments | Deixe vazio, ou veja abaixo |

A Pool URL precisa do prefixo: `stratum+tcp://` para pool, `grpc://` para node. Um `host:porta` sem
prefixo ou com `http://` é recusado.

Aplique o flight sheet. O painel mostra o hashrate quando as placas começam a minerar.

## Instalar pelo console da rig

```
/hive/miners/custom/custom-get https://github.com/fearke85/fckhash-miner/releases/download/v1.0.2/fckhash-1.0.2.tar.gz
```

Adicione `-f` para reinstalar.

## Atualizar para uma versão nova

Troque a versão na Installation URL, nos dois lugares: em `v1.0.2` e em
`fckhash-1.0.2.tar.gz`. O resto do flight sheet continua igual. Com a URL antiga, a rig
continua instalando a versão antiga.

Tudo o que o miner guarda (modelos, chaves, configurações, o log) fica em `/var/lib/fckhash`
entre atualizações e reinstalações, numa pasta por moeda. Os arquivos de escrow do Keryx de cada
carteira ficam em `keryx/escrow/<carteira>/` lá, e os modelos em `keryx/models/`.

Vindo da fear-miner: coloque a Installation URL nova e o miner name `fckhash`. A primeira
execução move `/var/lib/fear-miner` para `/var/lib/fckhash` e reorganiza a pasta sozinha; os
modelos não são baixados de novo. O que o miner não usa mais vai para `/var/lib/fckhash/old`:
apague essa pasta quando a rig estiver minerando como antes.

## Extra config arguments

Flags do miner levam um hífen. Flags da moeda levam dois. O Hash algorithm do flight sheet é a
moeda (`-coin`); um `-coin` em Extra config arguments substitui ele, para uma moeda que a HiveOS
não lista (Vela). Um `--` solto de uma ficha antiga ainda é aceito.

```
-device 0,2 -stats 0.0.0.0:4060 --tier 2
```

| O quê | Extra config arguments |
| --- | --- |
| Minerar só nas placas 0 e 2 | `-device 0,2` |
| Abrir a API de stats para a rede local | `-stats 0.0.0.0:4060` |
| Minerar Vela, que não está na lista de Hash algorithm da HiveOS | `-coin vela` |

O pacote já roda em modo headless. Opções do miner: [options.md](options.md). Opções da moeda: a
página dela ([Keryx](../../coins/keryx/docs/pt-BR/options.md), [Vela](../../coins/vela/README.pt-BR.md#opções-do-vela),
[Numen](../../coins/numen/README.pt-BR.md#opções-do-numen)).

## Onde fica cada coisa

| O quê | Onde |
| --- | --- |
| Log com avisos, erros e relatórios de crash | `/var/lib/fckhash/logs/log.txt` |
| Dados que o miner guarda | `/var/lib/fckhash` (escrow do Keryx: `keryx/escrow/`, faça backup) |
| Console do miner | `miner` no shell da rig, ou o log do miner no painel |

## Uso de CPU em rigs com poucos núcleos

Keryx: numa rig com muitas GPUs e CPU fraca, defina `FCKHASH_SYNC=block` no ambiente do flight sheet
para usar menos CPU, com cerca de 0,1% a menos de hashrate. `FCKHASH_SYNC=spin` usa um núcleo inteiro
por GPU.

O Vela deixa a CPU livre enquanto as placas trabalham e não lê `FCKHASH_SYNC`.
