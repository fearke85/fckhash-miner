# Linux

[English](../en/linux.md) · **Português (Brasil)** · [Voltar ao README](../../README.pt-BR.md)

Como instalar o FCKHash numa máquina Linux comum, fora do HiveOS, para qualquer moeda. Guia visual do
painel web integrado (`http://IP-DA-RIG:4455`): [Painel Web e Hub Multi-Rig](panel.md). Os
valores da sua moeda estão na página dela: [Keryx](../../coins/keryx/README.pt-BR.md#start-keryxsh-linux-e-start-keryxbat-windows), [Vela](../../coins/vela/README.pt-BR.md#start-velash-linux-e-start-velabat-windows), [Proxima](../../coins/proxima/README.pt-BR.md#start-proximash-linux-e-start-proximabat-windows), [NEB](../../coins/neb/README.pt-BR.md#start-nebsh-linux-e-start-nebbat-windows), [Numen](../../coins/numen/README.pt-BR.md#start-numensh-linux-e-start-numenbat-windows), [Vearl](../../coins/vearl/README.pt-BR.md#start-vearlsh-linux-e-start-vearlbat-windows).

## Antes de começar

- Linux x86-64 com glibc 2.35 ou mais nova (Ubuntu 22.04, Debian 12 ou mais novos).
- Driver NVIDIA 580 ou mais novo. Confira com `nvidia-smi`. O pacote traz as bibliotecas CUDA.
- Uma placa que a sua moeda suporte, e o espaço em disco que a página da moeda pede, ao lado do
  miner.

## Instalar

```
wget https://github.com/fearke85/fckhash-miner/releases/download/v1.0.0/fckhash-linux-1.0.0.tar.gz
tar -xzf fckhash-linux-1.0.0.tar.gz
cd fckhash
nano start-keryx.sh        # Vela: start-vela.sh
```

## Configurações no script de start

Cada moeda tem o seu script: `start-keryx.sh` para o Keryx, `start-vela.sh` para o Vela, `start-numen.sh` para o Numen.
Deixe uma linha sem comentário, a da pool ou a do solo, e coloque o seu endereço em `-wallet`. A pool, o node e os
outros valores estão na página da moeda.

As outras opções vão nessa mesma linha. Opções do miner: [options.md](options.md).

## Rodar

```
./start-keryx.sh           # Vela: ./start-vela.sh
```

Para o miner continuar rodando depois que você sair, inicie dentro do `screen` ou do `tmux`, ou
como serviço do systemd.

### O painel no menu de aplicativos

A primeira execução com um desktop adiciona o **FCKHash Panel** ao menu de aplicativos, com o
ícone do FCKHash. Ele abre o painel numa janela própria quando há Chrome, Edge, Brave ou Chromium
instalado; senão, no navegador padrão. O miner não precisa estar rodando: a janela então avisa que
a rig não responde.

Para o item voltar depois de mover a pasta ou mudar a `-panel`, rode:

```
./fckhash -shortcut
```

Para tirar, apague `~/.local/share/applications/fckhash-panel.desktop`.

O painel também instala como app pelo Chrome e pelo Edge: abra o painel e aperte **Instalar**.
Essa é a janela sem a barra do navegador. O Firefox não instala web apps, então nele o item do
menu de aplicativos é o caminho.

### Com sudo

As flags de overclock (`-cclock`, `-mclock`, `-lock-cclock`, `-lock-mclock`, `-fan`) precisam de
root:

```
sudo ./start-keryx.sh      # Vela: sudo ./start-vela.sh
```

Iniciado com `sudo` ou `su` a partir do seu próprio login, o miner cria os arquivos dele (a pasta
`data`) como o dono da pasta do miner, então você continua podendo editar e apagar. Ele
também devolve o que uma execução anterior como root deixou lá. Um serviço do systemd ou um login
de root mantém os arquivos como root; para pegá-los de volta, rode `sudo chown -R "$USER": .` na
pasta do miner.

## Atualizar para uma versão nova

Instale versões novas direto pelo [Painel Web](panel.md) (aperte **Instalar** no topo, ative o
**Auto-update**, ou use **Instalar em todas as rigs** a partir do Hub): o seu script de start, os
ajustes, o histórico e a pasta `./data` ficam intactos, e o miner volta sozinho para a versão
anterior se a nova não minerar.

Para atualizar à mão, extraia o pacote novo por cima da pasta antiga, mantendo o seu script de
start. O que o miner guarda fica em `./data` e é mantido.

Vindo da fear-miner: renomeie a pasta `fear-miner` para `fckhash` e extraia por cima dela. A
primeira execução move sozinha o que o miner guarda (`keryx`, `vela`, o log) para `./data`; os
modelos não são baixados de novo. O que ele não usa mais vai para `./data/old`: apague essa pasta
quando o miner estiver minerando como antes.

## Onde fica cada coisa

| O quê | Onde |
| --- | --- |
| Log com avisos, erros e relatórios de crash | `./data/logs/log.txt` |
| Dados que o miner guarda | `./data`, uma pasta por moeda. Faça backup de `./data/keryx/escrow`. Defina `FCKHASH_DATA_DIR` para mudar o local |
| Hashrate e shares | `curl 127.0.0.1:4060/stats` |
