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
