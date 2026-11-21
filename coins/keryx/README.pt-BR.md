# Keryx

[English](README.md) · **Português (Brasil)** · [Todas as moedas](../../README.pt-BR.md)

Como minerar Keryx com o FCKHash. Algoritmo: `keryxhash`.

> [!WARNING]
> **O hard fork H16** ativa no DAA 147.000.000 (07/11/2026, por volta das 11:00 de Brasília). A
> partir daí, versões anteriores à 1.6.0 são inválidas: a rede rejeita os shares e blocos delas.
> Quem minera solo precisa do keryx-node 1.6.7 ou mais novo.

## Placas

- NVIDIA Turing ou mais nova (RTX 20xx, 30xx, 40xx, 50xx) com 8 GB ou mais.
- NVIDIA CMP 170HX, inclusive as desbloqueadas para 64 GB, que mineram o tier 4. Medido: ~3,2 MH/s
  por placa com power limit de 200 W.
- Placas com 6 GB ou menos não são suportadas.
- Pascal (GTX 10xx, P104/P106) e anteriores não são suportadas.

## Qual tier minha placa minera?

Cada placa minera o maior tier cujo modelo cabe na memória dela com espaço para responder os
pedidos de IA desse tier. Não é preciso usar `--tier`.

| Memória da placa | Tier | Disco livre para o modelo |
| --- | --- | --- |
| 8-11 GB | 0 | ~6,7 GB |
| 12 GB | 1 | ~8,5 GB |
| 16-20 GB | 2 | ~10 GB |
| 24 GB | 3 | ~17 GB |
| 32 GB ou mais | 4 | ~30 GB |

Cada resposta de IA também fica guardada no disco por 12 horas, dezenas de MB por resposta, então
deixe alguns GB livres além do modelo num rig que responde muitos pedidos.

Com monitor ligado, o que o desktop ocupa não conta, e uma placa no limite pode minerar um tier
abaixo.

## Configurações

Instale o pacote pelo guia da sua plataforma e use estes valores.

### Flight sheet do HiveOS

| Campo | Valor |
| --- | --- |
| Miner name | `fckhash` |
| Installation URL | `https://github.com/fearke85/fckhash-miner/releases/download/v1.0.0/fckhash-1.0.0.tar.gz` |
| Hash algorithm | `keryxhash` |
| Wallet and worker template | Pool: `%WAL%.%WORKER_NAME%`. Solo: `%WAL%` |
| Pool URL | Pool: `stratum+tcp://krx.suprnova.cc:4404`, ou `stratum+ssl://krx.cryo-pool.com:5555`. Solo: `grpc://IP:PORTA` do seu node |
| Password | `x`, ou a senha da pool |

A carteira é o seu endereço Keryx, `keryx:...`. Guia de instalação: [HiveOS](../../docs/pt-BR/hiveos.md).

### start-keryx.sh (Linux) e start-keryx.bat (Windows)

A linha da pool já vem preenchida. Coloque o seu endereço em `-wallet`. Para solo, comente a
linha da pool e tire o comentário da linha solo.

| O quê | Valor |
| --- | --- |
| `-wallet` | Seu endereço Keryx, `keryx:...` |
| `-worker` | Um nome para esta rig na pool |
| `-pool` | `krx.suprnova.cc:4404` |
| `-node` | Só solo: `grpc://IP:PORTA` do seu node |
| `--escrow-cert` | Só solo, primeira execução: veja [mineração solo](docs/pt-BR/solo.md) |

### Pools

| Pool | Endereço | Transporte |
| --- | --- | --- |
| Suprnova | `krx.suprnova.cc:4404` (UE), `stratum-us.suprnova.cc:4404` (EUA) | simples; `stratum+ssl://krx.suprnova.cc:4405` também funciona |
| Cryo Pool | `krx.cryo-pool.com:5555` | só criptografado: `stratum+ssl://krx.cryo-pool.com:5555` |

O `-pool` aceita `host:porta` ou uma URL stratum; `stratum+ssl://` (ou `stratum+tls://`) conecta
com TLS. O miner confere o certificado da pool, então um endereço sem certificado válido não
conecta. A mesma carteira e o mesmo worker valem nas duas pools; cada uma tem o seu painel. No
HiveOS, coloque a URL completa no campo Pool URL.

Guias de instalação: [Linux](../../docs/pt-BR/linux.md) · [Windows](../../docs/pt-BR/windows.md).

## Proxima nas mesmas placas

Uma placa pode minerar Proxima junto com Keryx: o Keryx mantém quase toda a sua hashrate (98% por
padrão) e a Proxima minera o espaço que sobra. Basta somar quatro flags às configurações do Keryx;
nada mais muda. Placas RTX 50, no Linux e no HiveOS; o Windows vem depois. Num rig com outras
placas, elas mineram só Keryx.

| Flag | Valor |
| --- | --- |
| `-dual` | `proxima` |
| `-dual-pool` | `stratum+tcp://us3.innovlab.cc:19701` (`eu3.innovlab.cc:19701` na Europa), `http://prox.ariabrain.com:3359` (AriaPool) ou `stratum+ssl://proxi.suprnova.cc:3356` (Suprnova) |
| `-dual-user` | InnovLab: seu endereço Proxima. AriaPool: um nome para este rig. Suprnova: `CONTA.WORKER` |
| `-dual-password` | InnovLab / Suprnova: `x`. AriaPool: o token da sua conta |

No HiveOS mantenha a flight sheet como acima e coloque as flags em **Extra config arguments** (ou
ative a **Mineração dupla** na [aba Controle do Painel Web](../../docs/pt-BR/panel.md)):

```
-dual proxima -dual-pool stratum+tcp://us3.innovlab.cc:19701 -dual-user SEU_ENDERECO_PROXIMA.%WORKER_NAME%
```

Com `start-keryx.sh`, acrescente-as na linha da pool. Ali o `-worker` dá o
nome do rig nas duas pools: na InnovLab dê só o seu endereço Proxima, na AriaPool deixe o
`-dual-user` de fora, na Suprnova dê só a `CONTA`. Guia da Proxima sozinha:
[coins/proxima](../proxima/README.pt-BR.md).

- **Primeira partida:** cada placa mede como as duas moedas a dividem, cerca de 8 minutos,
  minerando as duas o tempo todo. O resultado fica guardado e só é medido de novo depois de uma
  atualização ou de um overclock novo.
- **Pedidos de IA:** a Proxima para durante cada resposta, e o Keryx responde com a placa inteira,
  como antes.
- **Tela e stats:** a hashrate e os shares da Proxima na tela do miner, ao lado dos do Keryx e por
  placa; `"dual"` no `/stats`; o segundo algoritmo no Hive.
- **Overclock:** o `-auto-oc` continua ajustando as placas que mineram só Keryx. Uma placa que
  minera duas moedas fica com os clocks que você der em `-lock-cclock` e `-cclock`, ou com os do
  driver. Duas moedas consomem mais energia e esquentam mais que o Keryx sozinho.
- **Quanto o Keryx mantém:** `-dual-keep 95` dá mais da placa à Proxima por até 5% da hashrate do
  Keryx. Opções: [opções do miner](../../docs/pt-BR/options.md#duas-moedas-nas-mesmas-placas).

## Pedidos de IA

O Keryx manda pedidos de IA aos mineradores de cada tier, e todo minerador daquele tier precisa
responder a tempo. O miner responde sozinho enquanto minera. Um pedido que a sua rig não responde
conta como strike contra o seu endereço de pagamento: mantenha o miner rodando e as placas
estáveis, e confira o `log.txt`. Mais em [solução de problemas](docs/pt-BR/troubleshooting.md).

Desde o H15 a rede dá bem menos tempo para responder. Um pedido que chega com o modelo
descarregado pode ser respondido com atraso.

A tela do miner mostra **AI processed**, as respostas que o seu pool ou nó aceitou, e no solo
**AI paid**, as recompensas de IA que a rede pagou à sua carteira e o valor em KRX. Uma recompensa
chega cerca de 12 horas depois da resposta; a que chega com o miner parado é paga, mas não entra
na contagem. As duas contagens ficam guardadas por carteira entre reinícios. Rigs que mineram para
a mesma carteira contam, cada uma, todas as recompensas dessa carteira.

## Mais

- [Mineração solo](docs/pt-BR/solo.md)
- [Opções do Keryx](docs/pt-BR/options.md) (depois de `--`)
- [Problemas do Keryx](docs/pt-BR/troubleshooting.md)
- [Opções do miner e problemas gerais](../../docs/pt-BR/options.md)
