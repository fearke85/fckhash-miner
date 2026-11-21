# Opções do miner

[English](../en/options.md) · **Português (Brasil)** · [Voltar ao README](../../README.pt-BR.md)

Opções que funcionam igual para todas as moedas. Guia visual do painel integrado:
[Painel Web e Hub Multi-Rig](panel.md). As opções de cada moeda estão na página dela:
[Keryx](../../coins/keryx/docs/pt-BR/options.md), [Vela](../../coins/vela/README.pt-BR.md#opções-do-vela),
[Proxima](../../coins/proxima/README.pt-BR.md#opções-da-proxima), [NEB](../../coins/neb/README.pt-BR.md#opções-da-neb),
[Numen](../../coins/numen/README.pt-BR.md#opções-do-numen), [Vearl](../../coins/vearl/README.pt-BR.md#opções-do-vearl).

## Onde colocar as opções

| Plataforma | Onde |
| --- | --- |
| HiveOS | Flight sheet, **Extra config arguments** |
| Linux | Na linha do `start-<moeda>.sh` da moeda |
| Windows | Na linha do `start-<moeda>.bat` da moeda |

Flags do miner levam um hífen. Flags da moeda levam dois. A ordem não importa: tudo fica na
mesma linha. Um `--` solto de uma linha antiga ainda é aceito e ignorado.

```
-device 1,6 -stats 0.0.0.0:4060 --tier 2
└──── flags do miner ──────────┘ └ moeda ┘
```

Caminhos são uma palavra só, sem aspas.

## Opções

| Opção | O que faz | Padrão |
| --- | --- | --- |
| `-coin keryxhash` | Moeda a minerar: `keryxhash`, `vela`, `proxima`, `neb`, `numen` ou `vearl`. Igual ao positional; os dois juntos, vale `-coin` | obrigatório |
| `-device 0,2,5` | Minera só nessas placas, pelos números que o miner mostra | `all` |
| `-pool host:porta` | Pool onde minerar: `host:porta`, `stratum+tcp://host:porta` (simples) ou `stratum+ssl://host:porta` (criptografado) | definido pelo lançador |
| `-node grpc://IP:PORTA` | Seu node, para minerar solo | definido pelo lançador |
| `-wallet ENDERECO` | Endereço de pagamento. `-address` é o mesmo campo; vale o último | definido pelo lançador |
| `-worker NOME` | Nome da rig. Substitui o trecho da carteira depois do primeiro `.` | |
| `-user ENDERECO.WORKER` | Usuário completo da pool. Prefira `-wallet` e `-worker` | definido pelo lançador |
| `-password x` | Senha da pool | `x` |
| `-stats ENDERECO:PORTA` | Endereço da API de stats (`GET /stats`) | `127.0.0.1:4060` |
| `-panel ENDERECO:PORTA` | Endereço do painel, ou `off` | `0.0.0.0:4455` |
| `-open` | Abre o painel numa janela própria e encerra; é o que o item do menu de aplicativos roda | |
| `-shortcut` | Grava de novo o item do painel no menu de aplicativos e encerra | |
| `-log-file CAMINHO` | Onde gravar o `log.txt` | `logs/log.txt` na pasta de dados |
| `-update manual` | Procura uma versão nova do FCKHash a cada 3 horas. `manual`: avisa e instala quando você aperta Instalar no painel; `auto`: instala sozinho; `off`: nunca procura | `manual` |

Dois hífens ainda funcionam nestas flags (`--pool`, `--device`). Flags da moeda não: `--escrow-dir`
é flag da moeda, `-escrow-dir` é recusado.

A API de stats não tem senha. `0.0.0.0:4060` abre para qualquer máquina que alcance a rig, então
mantenha ela só na sua rede local.

O painel também não tem senha e, por padrão, qualquer máquina da rede da rig abre ele
(`http://IP-DA-RIG:4455`). Pela Tailscale, use o endereço `100.x` da rig. Numa rig que a internet
alcança, passe `-panel 127.0.0.1:4455` ou `-panel off`.

A aba Controle do painel muda a moeda, a pool, a carteira, as placas que mineram, as opções da
moeda, o auto OC e a mineração dupla, e reajusta uma placa; o miner reinicia para aplicar cada mudança. Qualquer pessoa que abra o
painel pode mudar esses ajustes, inclusive a carteira: numa rede compartilhada, feche o painel
como acima. O que o painel salva vale por cima da linha de start (flight sheet ou arquivo de
start) até a linha de start mudar: aí a linha de start vale e os ajustes do painel são apagados.
Uma versão nova do FCKHash mantém esses ajustes.

A cada 3 horas o miner pergunta à página pública de releases do FCKHash se saiu uma versão nova.
Ele não envia nada sobre a rig. Quando sai, o log avisa e a aba Controle mostra a versão com as
novidades.

- No HiveOS, coloque na flight sheet a Installation URL que a aba Controle mostra e aplique.
- No Linux e no Windows, aperte Instalar na aba Controle, ou escolha "Instalar sozinho"
  (`-update auto`). O miner baixa a versão nova, confere e reinicia com ela quando nenhum pedido
  de IA estiver rodando. Seu arquivo de start, ajustes, histórico e dados ficam como estão. Se a
  versão nova não minerar na rig, a anterior volta sozinha e o painel avisa; essa versão não é
  oferecida de novo.
- A pasta do miner precisa poder ser gravada pelo usuário que roda o miner. Se não puder, instale
  à mão: baixe o pacote e descompacte por cima da pasta, mantendo seu arquivo de start.

`-update off` desliga a busca.

A aba Rigs conectadas adiciona o painel de outra rig pelo endereço (`192.168.0.60:4455`, ou o
endereço ou nome dela na Tailscale). O painel passa a ler essa rig a cada 5 segundos, guarda o
histórico dela e mostra ela em todas as abas: escolha a rig na barra lateral. A rig adicionada
precisa ter o painel acessível a partir deste.

O botão Exportar logs da aba Logs salva um zip com os logs da rig, as placas, processador,
memória, placa-mãe e discos, os erros recentes do sistema e as leituras dos últimos dois dias.
Envie junto com um relato de bug. Senhas e tokens são retirados; a carteira e a pool ficam.

## Overclock

O miner ajusta clocks e ventoinhas por conta própria, no Linux e no Windows, pelo driver da
NVIDIA (NVML).

| Opção | O que faz |
| --- | --- |
| `-cclock 275` | Offset do clock do core em MHz. Negativo diminui: `-cclock -100` |
| `-mclock 1000` | Offset do clock da memória em MHz. Negativo diminui: `-mclock -500`. O clock de memória que o miner mostra muda metade do valor |
| `-lock-cclock 2610` | Trava o clock do core nesse MHz |
| `-lock-mclock 810` | Trava o clock da memória nesse MHz (RTX 30 em diante) |
| `-pl 250` | Power limit em watts. A placa segura o limite mais próximo que aceita |
| `-fan 75` | Velocidade da ventoinha em %, todas as ventoinhas da placa |

Um valor só vale para todas as placas. Placas diferentes recebem uma lista, um valor por placa,
na ordem do `-device`; `*` deixa aquela placa como está:

```
-device 0,1,2 -cclock 275,250,* -lock-cclock 2610,1350,1350 -fan 75
```

Aqui a placa 0 recebe +275 e 2610 MHz, a placa 1 +250 e 1350 MHz, a placa 2 mantém o offset e
recebe 1350 MHz, e todas as ventoinhas ficam em 75%. Sem `-device`, a lista segue os números de
placa que o miner mostra: 0, 1, 2…

- Uma lista com a quantidade errada de valores não muda nada em nenhuma placa, e o log avisa.
  Se uma placa cair do barramento, os números mudam; aí o miner minera no stock em vez de
  colocar o clock de uma placa em outra.
- No Linux precisa de root ([com sudo](linux.md#com-sudo)) e no Windows de **Executar como
  administrador**. O HiveOS já roda o miner como root.
- Ao sair (Ctrl+C, ou fechando a tela), o miner desfaz o que mudou: offsets 0, clocks
  destravados, power limit de volta ao da placa, ventoinhas no automático. Se o processo for morto, os ajustes ficam até o
  próximo start.
- No HiveOS, use estas flags ou a página de OC para uma placa, não as duas: a saída do miner
  desfaz o lock e a ventoinha do Hive até o Hive aplicar o perfil de novo.

A aba Controle do painel ajusta o mesmo que a página de OC do HiveOS, para todas as placas de uma
vez ou placa por placa, e guarda por moeda: a moeda que minera usa o dela quando o miner inicia. É o
modo Manual do painel Overclock: vale com o overclock automático desligado. Enquanto o miner roda no
modo Manual, mudar o overclock manual da moeda em execução aplica os novos clocks nas placas na
mesma hora, sem reiniciar o miner. No Linux e no Windows com desktop, ligar o overclock pede
permissão de root / Administrador automaticamente quando o miner foi iniciado como usuário comum.
Uma moeda sem nada ajustado ali roda as flags acima.

## Overclock automático

Suportado no **Keryx** (todas as placas suportadas) e na **Proxima** (placas RTX 30 e RTX 50); as
outras moedas mineram com o clock que a placa já tem.

| Opção | O que faz |
| --- | --- |
| `-auto-oc` | O miner encontra o clock do core, o undervolt, a memória e a ventoinha de cada placa, e guarda |
| `-eco` | Com `-auto-oc`, o padrão: o melhor hashrate por watt, placas a no máximo 70 °C |
| `-boost` | Com `-auto-oc`: mais hashrate enquanto a energia extra compensa, placas a no máximo 75 °C |
| `-retune` | Com `-auto-oc`: ajusta todas as placas de novo. `-retune 1,3` só as placas 1 e 3 |
| `-max-fan` | Com `-auto-oc`: o máximo de ventoinha que o miner pode usar, em porcentagem. Padrão 100. `-max-fan 80,*,100` por placa |

```
-auto-oc              eco em todas as placas
-auto-oc -boost       boost em todas as placas
-auto-oc -retune 2    eco, e ajusta a placa 2 de novo
-auto-oc -max-fan 80  eco, e a ventoinha nunca passa de 80%
```

- Todas as placas são ajustadas ao mesmo tempo; a rig inteira leva uns 30 a 45 minutos. O
  hashrate varia enquanto isso; a TUI mostra `AUTO OC eco · tuning 4 of 4` (placas ainda em
  ajuste) e um `~` ao lado do hashrate de cada placa em medição. Deixe a rig minerando até o
  cabeçalho mostrar `tuned`.
- O resultado fica salvo por placa, e o próximo start já usa. Uma placa nova ou um driver novo
  é ajustado de novo sozinho. O `-retune` ajusta uma placa de novo quando você quiser.
- O miner controla a ventoinha. Com `-fan`, a placa fica na sua ventoinha e o ajuste trabalha
  com ela. Acima do limite de temperatura a ventoinha sobe primeiro, até o `-max-fan` (100% por
  padrão); só nesse limite o clock desce. O `-max-fan` não pode ser usado junto com `-fan`.
  Mudar o `-max-fan` ajusta a placa de novo na ventoinha nova, porque as temperaturas antigas
  foram medidas com outra refrigeração.
- Nas placas que informam a temperatura da memória (RTX 30 com GDDR6X, RTX 50, CMP 170HX e
  outras placas HBM), a memória tem um limite próprio, o mesmo no `-eco` e no `-boost`: 5 °C
  abaixo da temperatura em que ela se limita sozinha. A GDDR6X se limita a 110 °C (memória a no
  máximo 105 °C), a GDDR7 a 105 °C (100 °C); as placas HBM usam o limite que o driver informa. Quando a memória passa do limite, desce o clock da
  memória, não o do core. O log mostra o limite de memória de cada placa no início. Uma placa
  que passa a informar a temperatura da memória é ajustada de novo uma vez.
- Um ajuste que derrubou o miner ou devolveu resultados errados nunca é tentado de novo. O
  ajuste pode reiniciar o miner algumas vezes; é esperado.
- Uma placa com `-cclock`, `-mclock`, `-lock-cclock`, `-lock-mclock` ou `-pl` fica por sua conta e
  aparece como `manual`; as outras são ajustadas.
- Enquanto uma placa responde a um pedido de IA, ela roda nos clocks do driver; assim que volta
  a minerar, o overclock (seu ou do `-auto-oc`) volta. Só os clocks que o miner definiu são
  trocados; uma placa só com `-fan`, ou com OC do Hive, fica como está.
- Isso vale também para `-boost` e clocks manuais: um overclock agressivo pode derrubar a
  geração de tokens, então os clocks do driver são a base segura para toda resposta.
- Tire o `-retune` quando o ajuste terminar: com ele, todo start ajusta de novo.
- As mesmas permissões das flags acima: root no Linux, **Executar como administrador** no
  Windows.
- **HiveOS:** com `-auto-oc`, deixe o OC e o autofan da rig vazios no Hive. Os dois mexeriam
  nos mesmos clocks e ventoinhas, e vale o último aplicado.

## Duas moedas nas mesmas placas

O Keryx pode dividir cada placa com a Proxima ([como configurar](../../coins/keryx/README.pt-BR.md#proxima-nas-mesmas-placas)).
A primeira moeda é a que o `-coin` indica; a segunda usa as flags `-dual-*`.

| Opção | O que faz | Padrão |
| --- | --- | --- |
| `-dual proxima` | Minera uma segunda moeda nas mesmas placas, ao mesmo tempo | |
| `-dual-pool URL` | A pool da segunda moeda, como `-pool` | obrigatória com `-dual` |
| `-dual-user NOME` | O usuário da segunda moeda na pool. O `-worker` é somado a ele como à carteira, ou é o nome inteiro quando o `-dual-user` fica de fora | |
| `-dual-password TOKEN` | A senha ou o token da segunda moeda na pool | `x` |
| `-dual-keep 98` | A parte da hashrate, em porcento, que a primeira moeda mantém; a segunda minera o resto da placa | `98` |
| `-dual-sms 16` | Um número fixo de SMs de cada placa para a segunda moeda, em vez de medir | |

## Variáveis de ambiente

| Variável | O que faz |
| --- | --- |
| `FCKHASH_DATA_DIR` | Pasta de tudo o que o miner guarda (modelos, escrow, configurações, o log). Padrão `data` ao lado do miner; `/var/lib/fckhash` no HiveOS |
| `FCKHASH_SYNC=block` | Keryx: menos uso de CPU em rigs com muitas GPUs e poucos núcleos, com cerca de 0,1% a menos de hashrate |
| `FCKHASH_SYNC=spin` | Keryx: um núcleo inteiro de CPU por GPU. O Vela não lê `FCKHASH_SYNC` |

## Exemplos

| Objetivo | Extra config no HiveOS | `start-<moeda>.sh` / `start-<moeda>.bat` |
| --- | --- | --- |
| Só as placas 0 e 1 | `-device 0,1` | `-device 0,1` na linha de start |
| Stats na rede local | `-stats 0.0.0.0:4060` | `-stats 0.0.0.0:4060` na linha de start |
| Dados em outro disco | | `FCKHASH_DATA_DIR=/data/fckhash` |
| Overclock igual em todas as placas | `-cclock 275 -lock-cclock 2610 -lock-mclock 810 -fan 75` | o mesmo, na linha de start |
