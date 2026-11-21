# Proxima

[English](README.md) · **Português (Brasil)** · [Todas as moedas](../../README.pt-BR.md)

Mineração de Proxima (PROX) com o FCKHash. Algoritmo: `proxima` (`prox` no HiveOS). Mineração em
pool na **InnovLab**, **AriaPool** ou **Suprnova**, solo como seeker no seu próprio node
(`proxi node mine`), ou em **mineração dupla nas mesmas placas junto com o Keryx** (RTX 50).

O miner mostra a velocidade em **GH/s** e a eficiência em **MH/J**.

## Placas

- NVIDIA Turing ou mais nova (RTX 20xx, 30xx, 40xx, 50xx).
- Pascal (GTX 10xx) e anteriores não são suportadas.
- **Overclock automático (`-auto-oc`):** suportado nas placas RTX 30 e RTX 50 ao minerar Proxima
  sozinha. A velocidade da Proxima depende apenas do clock do core, então o auto overclock trava a
  memória no estado de menor consumo (`405 MHz`, economizando de 14 a 47 W por placa nas RTX 30) e
  ajusta o lock e o undervolt do core para a melhor eficiência (`-eco`, padrão) ou velocidade
  (`-boost`).

## Configuração

Instale o pacote seguindo o guia da sua plataforma e configure pelo
[Painel Web](../../docs/pt-BR/panel.md) (`http://IP-DA-RIG:4455`) ou use os valores abaixo.

### Pools

| Pool | `-pool` no Linux e Windows | Pool URL no HiveOS | Login (`-wallet` / `-user`) | Senha (`-password`) |
| --- | --- | --- | --- | --- |
| **InnovLab (US)** | `stratum+tcp://us3.innovlab.cc:19701` | `stratum+tcp://us3.innovlab.cc:19701` | Seu endereço Proxima (`.WORKER`) | `x` (não precisa de token) |
| **InnovLab (EU)** | `stratum+tcp://eu3.innovlab.cc:19701` | `stratum+tcp://eu3.innovlab.cc:19701` | Seu endereço Proxima (`.WORKER`) | `x` (não precisa de token) |
| **AriaPool** | `http://prox.ariabrain.com:3359` | `http://prox.ariabrain.com:3359` | Nome do worker (`rig1`) | Token da sua conta na AriaPool |
| **AriaPool (Solo)** | `http://prox.ariabrain.com:3369` | `http://prox.ariabrain.com:3369` | Nome do worker (`rig1`) | Token da sua conta na AriaPool |
| **Suprnova (SSL)** | `stratum+ssl://proxi.suprnova.cc:3356` | `stratum+ssl://proxi.suprnova.cc:3356` | `CONTA.WORKER` | `x`, ou a senha do worker |

### Flight sheet no HiveOS

| Campo | Valor |
| --- | --- |
| Miner name | `fckhash` |
| Installation URL | `https://github.com/fearke85/fckhash-miner/releases/download/v1.0.0/fckhash-1.0.0.tar.gz` |
| Hash algorithm | `proxima` (ou coloque `-coin proxima` em Extra config arguments) |
| Wallet and worker template | InnovLab / Suprnova: `%WAL%.%WORKER_NAME%`. AriaPool: `%WORKER_NAME%` |
| Pool URL | `stratum+tcp://us3.innovlab.cc:19701` (ou qualquer pool da tabela acima) |
| Password | InnovLab / Suprnova: `x`. AriaPool: token da sua conta |
| Extra config arguments | `-coin proxima` (adicione `-auto-oc` em rigs RTX 30 / RTX 50) |

Guia de instalação: [HiveOS](../../docs/pt-BR/hiveos.md).

### start-proxima.sh (Linux) e start-proxima.bat (Windows)

A linha da pool InnovLab já vem pronta. Coloque o seu endereço Proxima em `-wallet`:

| O quê | Valor |
| --- | --- |
| `-wallet` | Seu endereço Proxima (na Suprnova, sua conta; na AriaPool, omita e use `-worker`) |
| `-worker` | Um nome para esta rig na pool |
| `-pool` | `stratum+tcp://us3.innovlab.cc:19701` |
| `-password` | `x` na InnovLab e Suprnova; token da sua conta na AriaPool |

Guias de instalação: [Linux](../../docs/pt-BR/linux.md) · [Windows](../../docs/pt-BR/windows.md).

### Mineração dupla junto com o Keryx (RTX 50)

Placas RTX 50 no Linux e no HiveOS podem minerar Proxima nas mesmas placas ao mesmo tempo que o
Keryx: o Keryx mantém 98% do seu hashrate por padrão e a Proxima usa o espaço livre da placa. Ative
a **Mineração dupla** na [aba Controle do Painel Web](../../docs/pt-BR/panel.md#controle-overclock-manual-ao-vivo-e-auto-oc)
ou veja a [configuração de mineração dupla na página do Keryx](../keryx/README.pt-BR.md#proxima-nas-mesmas-placas).

### Solo no seu próprio node Proxima (`proxi node mine`)

Inicie o `proxi node mine --workers 0` com o listener de seeker ativo (`localhost:18555` e um token
configurado no perfil do node) e aponte o FCKHash para ele:

```
./fckhash -coin proxima -node localhost:18555 -password SEU_TOKEN_DO_SEEKER
```

## Opções da Proxima

Opções da moeda levam dois hífens. Normalmente não são necessárias, pois `-user` / `-worker` e
`-password` já definem o nome e o token:

| Opção | O que faz |
| --- | --- |
| `--token TOKEN` | Token da AriaPool ou do `proxi node mine` (igual a `-password TOKEN`) |
| `--name NOME` | Nome do seeker ou worker exibido pelo node ou pool (igual a `-user` / `-worker`) |

## Ajuda

- [Discord](https://discord.gg/uK93NPAhbd)
- [Abra uma issue](https://github.com/fearke85/fckhash-miner/issues/new/choose) e anexe a exportação de logs do painel ou o `log.txt`
