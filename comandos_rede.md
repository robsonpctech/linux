### IP Duplicado

> O arp-scan é uma ferramenta de linha de comando que usa o protocolo ARP para descobrir e identificar máquinas IP na rede local. 
```
sudo apt install arp-scan
```
Liste os IPs
```
sudo arp-scan -I nome da interface de rede -l
```
Bloquear os IPs
```
sudo arp -s digite o ip duplicado 0
```
Desbloquear os IPs
```
sudo arp -s digite o ip duplicado
```

### Listar IPs Livres e Ocupados com Nmap
> Nmap é um software de varredura e mapeamento de rede

Lista IPs Livres
```
nmap -v -sn -oG - digite o IP/24 | grep 'Down'
```

Lista IPs Ocupados
```
nmap -v -sn -oG - digite o IP/24 | grep 'Up'
```

### Desabilitando client DNS systemd-resolved
> Procedimento para desabilitar o systemd-resolved e utilizar o NetworkManager para gerenciar a resolução DNS nas estações.

Edite o arquivo NetwokManager.conf
```
sudo nano /etc/NetworkManager/NetworkManager.conf
```
Adicione a opção dns=default como mostrado abaixo
```
[main]
plugins=ifupdown,keyfile
dns=default
```
Remova o arquivo resolv.conf será criado outro automaticamente após reiniciar o serviço.
```
sudo rm -rf /etc/resolv.conf
```
Retire o systemd-resolved da inicialização do sistema
```
sudo systemctl disable systemd-resolved
```
Pare o serviço systemd-resolved
```
sudo systemctl stop systemd-resolved
```
Reinicie o NetworkManager para gerar um novo resolv.conf e assim gerenciar a resolução DNS
```
sudo systemctl restart network-manager
```
### DHCP
> Procedimento para configurar o dhcp

entre em cd /etc/network
e deixe somente esses arquivos abaixo, o que tiver a mais apague.
```
if-down.d
if-post-down.d
if-pre-up.d
if-up.d
```
entre em cd /etc/netplan
e deixe somente esses arquivos abaixo, o que tiver a mais apague
```
01-network-manager-all.yaml
```
o conteudo desse arquivo acima é:
```
network:
  version: 2
  renderer: NetworkManager
```
entre em cd /etc/udev/rules.d
e deixe somente esses arquivos abaixo, o que tiver a mais apague
```
70-snap.firefox.rules
70-snap.snapd.rules
```
### IP Fixo

**Primeiro passo**
```
cd /etc/netplan
mv 00-installer-config.yaml 00-installer-config.yaml.old
nano rede_local.yaml
```
coloque o conteudo abaixo, alterado o ip, macadress e nome da placa de rede dentro do arquivo rede_local.yaml
```
/*This is the network config written by 'subiquity': */
network:
  version: 2
  renderer: networkd
  ethernets:
    nome da interface de rede:
      match:
        macaddress: "macadress"
      set-name: local0
      addresses: [ IP/24 ]
      nameservers:
          search: [ dominio ]
          addresses:
              - "ip de broadcast"

      routes:
        - to: default
          via: ip de broadcast
```
**Segundo Passo**
```
cd /etc/network
mkdir interfaces.d
cd interfaces.d
```
dentro de interfaces.d crie o arquivo:
```
nano local0
```
com o conteúdo abaixo, isso fará com que deixe de ser dhcp e passe a ser ip fixo. (mude o ip para o ip que você deseja).
```
auto local0
iface local0 inet static
        address IP/24
        gateway ip de broadcast
        dns-search digite o domínio
        dns-nameservers digite o ip de broadcast
```
**Terceiro Passo**

Entre em /etc/network e crie um arquivo chamado interfaces com o conteúdo abaixo
```
nano interfaces
```
```
/* The loopback network interface */
auto lo
iface lo inet loopback

source /etc/network/interfaces.d/*
```

**Quarto Passo**

entre em /etc/udev/rules.d/ e crie o arquivo abaixo:
```
nano 70-persistent-net.rules
```
cole o conteúdo abaixo mudando só o mac address
```
SUBSYSTEM=="net", ACTION=="add", ATTR{address}=="macadress", NAME="local0"
```
**Quinto Passo (opcional)**

Altere o nome do servidor:
```
nano /etc/hostname
nano /etc/hosts
```

**Sexto Passo**
```
reboot
```
