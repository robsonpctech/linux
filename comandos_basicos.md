## CP
> Copia arquivos e diretórios
```
cp -r -p -v  caminho de origem caminho de destino
```
**Legenda**
```
-r = recursivo
-p = progressivo
-v = verboso
```

## MV - usado para mover e para renomear
Mover
```
mv arquivo.txt /home/joaozinho/Documentos
```

Renomear
```
mv nome_antigo.txt nome_novo.txt
```

## RSYNC
> Significa “sincronização remota”, é uma ferramenta de sincronização remota e local. Ele usa um algoritmo que minimiza a quantidade de dados copiados ao mover apenas as porções de arquivos que foram alteradas.

copia os arquivos e não as pastas da origem para o destino

```
rsync origem/* destino/
```

copia tudo da origem para o destino

```
rsync -varh origem/* destino/
```

sincroniza tudo da origem para o destino, apagando tudo que estiver no destino

```
rsync -av --delete  origem/ destino/
```

sincronize a origem com o destino ou vice versa com barra de progresso

```
rsync -azP origem destino
```

copiando na rede

```
rsync -varhzP origem usuario@ip de destino:/destino
```

Exemplo prático

```
rsync --progress -aruv nome do pc::caminho de destino/* .
```

**Legenda**
```
. = significa tudo
-v = verboso, mostra o progresso, preserva permissões
-a = inclui as pastas
-r = recursivo
-h = compreensivel para humanos
-z = é para comprimir
-P = barra de progresso
```

## SCP
> Significa (Secure Copy), ferramenta do linux que realiza transferência de arquivos remotos entre servidores e utiliza conexão SSH. 

```
scp -rpv caminho de origem nome_do_usuário@digite o caminho do arquivo de destino:caminho de destino
```

**Legenda**
```
-r = recursivo
-p = progressivo
-v = verboso
```

## DD
> dd ou “data duplicator” é uma ferramenta que te ajuda a converter, copiar e criar novos arquivos em formato de imagem, tanto em discos rígidos, quanto em outros arquivos

```
dd if=origem of=destino bs=4M conv=noerror,sync status=progress
```

Copia de HD normal windows para HD ssd
```
- desfragmente o disco
- faça o resize no terminal ou no gpated
- depois faça a copia
```

## Upgrade de Sistema Operacional

verifique a versão do seu S.O
```
hostnamectl
```

atualize e corrija possíveis problemas no seu S.O antes de fazer o upgrade
```
apt update -y && apt upgrade -y && apt autoremove -y && apt-get update --fix-missing  -y && apt autoclean -y && dpkg --configure -a && apt --fix-broken install -y && apt -f install -y
```

cheque se tem atualização disponível

```
sudo do-release-upgrade --check-dist-upgrade-only -d
```
atualize
```
sudo do-release-upgrade
```
Caso apareça algum erro depois do comando acima, edite o arquivo abaixo e mude a opção para: normal
em seguida volte a fazer o comando de atualização

```
nano /etc/update-manager/release-upgrades
```

## Montar e Desmontar Pendrive / Cartão SD
> passo a passo simples de montar um pendrive ou cartão SD.

```
mkdir /mnt /SD
```
Nessa parte do exemplo abaixo tem escrito vfat que pode ser substituído por ext4 ou qualquer outro formato.
Assim como sdc1 é partição que também pode ser substituída pela partição do dispositivo.

exemplo:

```
mount -t vfat /dev/sdc1 /mnt /SD
mount -t ext4 /dev/sdd3 /mnt /SD
cd /mnt /SD
```

Como eu vou saber qual é a partição do meu dispositivo ?

```
lsblk -f
ls -1rt /dev/sd*
parted -l | grep '/dev/sd'
```

**O que cada comando listado faz ?**

ls -1rt /dev/sd*

```
1 - liste um por linha
r - reverso
t - em ordem do mais novo p/ mais velho
```

parted -l | grep '/dev/sd'

manipula partições do disco

```
parted -l -----------> lista as partições
| grep '/dev/sd' ----> filtra os dados de interesse que estão entre aspas
```

## SSH pelo pen-drive de boot
> esses passos facilitam a configuração remota em um ambiente de testes e pré-instalação de sistema operacional

```
apt-get install openssh-server -y
passwd root
senha digite a senha
```

acesse o arquivo /etc/ssh/sshd_config
Procura essa linha aqui:
```
#PermitRoot Login prohibit-password
```
tira o #
apaga o prohibit-password e coloca yes no lugar
vai ficar assim
```
PermitRootLogin yes
```
salva, sai e reinicia o ssh
```
/etc/init.d/ssh restart
```

ao tentar logar faça:
```
ssh root@ip da maquina
senha digite a senha
```
