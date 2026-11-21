# Windows

[English](../en/windows.md) · **Português (Brasil)** · [Voltar ao README](../../README.pt-BR.md)

Como instalar o FCKHash no Windows, para qualquer moeda. Guia visual do painel web integrado
(`http://127.0.0.1:4455` ou `http://IP-DA-RIG:4455`): [Painel Web e Hub Multi-Rig](panel.md). Os
valores da sua moeda estão na página dela: [Keryx](../../coins/keryx/README.pt-BR.md#start-keryxsh-linux-e-start-keryxbat-windows), [Vela](../../coins/vela/README.pt-BR.md#start-velash-linux-e-start-velabat-windows), [Proxima](../../coins/proxima/README.pt-BR.md#start-proximash-linux-e-start-proximabat-windows), [NEB](../../coins/neb/README.pt-BR.md#start-nebsh-linux-e-start-nebbat-windows), [Numen](../../coins/numen/README.pt-BR.md#start-numensh-linux-e-start-numenbat-windows), [Vearl](../../coins/vearl/README.pt-BR.md#start-vearlsh-linux-e-start-vearlbat-windows).

## Antes de começar

- Windows 10 ou 11, 64 bits.
- Driver NVIDIA 580 ou mais novo, do site da NVIDIA ou do NVIDIA app. O pacote traz as
  bibliotecas CUDA.
- Uma placa que a sua moeda suporte, e o espaço em disco que a página da moeda pede, ao lado do
  miner.

## Instalar

1. Baixe o `fckhash-windows-1.0.1.zip` na
   [página da release](https://github.com/fearke85/fckhash-miner/releases/tag/v1.0.1).
2. Extraia numa pasta só dele, por exemplo `C:\fckhash`.
3. Clique com o botão direito no `start-keryx.bat` (Vela: `start-vela.bat`) e escolha **Editar** (ou abra no Bloco de Notas).

## Configurações no script de start

Cada moeda tem o seu script: `start-keryx.bat` para o Keryx, `start-vela.bat` para o Vela, `start-numen.bat` para o Numen.
Deixe uma linha sem comentário, a da pool ou a do solo, e coloque o seu endereço em `-wallet`. A pool, o node e os
outros valores estão na página da moeda.

As outras opções vão nessa mesma linha. Opções do miner: [options.md](options.md).

## Rodar

Salve o script e dê dois cliques nele. Deixe a janela aberta: fechar a janela para o miner.
Quando o CUDA morre numa placa, o miner sai e o script inicia ele de novo depois de 3 segundos.

### O painel no menu Iniciar

A primeira execução adiciona o **FCKHash Panel** ao menu Iniciar, com o ícone do FCKHash. Ele abre
o painel no navegador padrão; o miner não precisa estar rodando, e o navegador então avisa que o
endereço não respondeu.

Para o item voltar depois de mover a pasta ou mudar a `-panel`, rode num Prompt de Comando:

```
fckhash.exe -shortcut
```

Para tirar, apague o **FCKHash Panel** do menu Iniciar.

Para uma janela sem a barra do navegador, instale o painel como app: abra no Edge ou no Chrome e
aperte **Instalar**. É esse item que você fixa na barra de tarefas.

## Atualizar para uma versão nova

Instale versões novas direto pelo [Painel Web](panel.md) (aperte **Instalar** no topo, ative o
**Auto-update**, ou use **Instalar em todas as rigs** a partir do Hub): o seu arquivo de start, os
ajustes, o histórico e a pasta `data` ficam intactos, e o miner volta sozinho para a versão anterior
se a nova não minerar.

Para atualizar à mão, extraia o zip novo por cima da pasta antiga e escolha **Ignorar** (**Skip**)
ao perguntar se deseja substituir o seu `start-*.bat`. O que o miner guarda fica em `data` e é
mantido.

Vindo da fear-miner: renomeie a pasta `fear-miner` para `fckhash` e extraia por cima dela. A
primeira execução move sozinha o que o miner guarda (`keryx`, `vela`, o log) para `data`; os
modelos não são baixados de novo. O que ele não usa mais vai para `data\old`: apague essa pasta
quando o miner estiver minerando como antes.

## Onde fica cada coisa

| O quê | Onde |
| --- | --- |
| Log com avisos, erros e relatórios de crash | `data\logs\log.txt` ao lado do `fckhash.exe` |
| Dados que o miner guarda | `data`, uma pasta por moeda. Faça backup de `data\keryx\escrow`. Defina `FCKHASH_DATA_DIR` para mudar o local |
| Hashrate e shares | `http://127.0.0.1:4060/stats` no navegador |
