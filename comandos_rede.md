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
