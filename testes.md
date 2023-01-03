### Teste de Memória
> comando dmidecode nos fornece o tipo, a velocidade e a quantidade máxima de memória possível no nosso hardware, bem como a quantidade de slots presentes (vazios e ocupados) etc

O manual descreve os tipos 5, 6, 16 e 17 como tipos de informação acerca da memória.

- 5 - controlador de memória.
- 6 - módulo de memória.
- 16 - array físico de memória.
- 17 - dispositivo de memória.

```
dmidecode -t 16
dmidecode -t 17
```
Clique asseguir para acesso [avançado](https://www.redhat.com/sysadmin/linux-tools-dmidecode) ao dmidecode. 

### Teste de HD
> Smartmontools (S.M.A.R.T. Monitoring Tools) é um conjunto de programas utilitários (smartctl e smartd) para controlar e monitorar sistemas de armazenamento de computador.

Instalação:
```
apt install smartmontools
```
Analizando o HD
```
smartctl --all /dev/nome do sda 
```
Mais Opções de teste
```
smartctl -H /dev/sda ---------- (resultado se  passou)
smartctl -t short /dev/sda ---- (teste curto)
smartctl -t long /dev/sda  ---- (teste longo)
smartctl -l selftest /dev/sda --(acompanha o status do scanner)
smartctl -h ------------------- (ajuda de comandos)
```
Clique asseguir para acesso [avançado](https://help.ubuntu.com/community/Smartmontools) ao Smartmontools.

### Teste de Stress do Computador
> O stress é usado para testar processador, placa-mãe, hd, memória e demais componentes

Instalação
```
apt install stress
apt install stress-ng
```
Executa por 5 minutos, 4 trabalhos na cpu, 2 trabalhos de IO, usa 16G de ram.
```
stress-ng --cpu 4 --io 2 --vm 4 --vm-bytes 16G --timeout 300s --metrics-brief
```
Clique asseguir para acesso [avançado_01](https://ironlinux.com.br/estressando-mem-disco-e-cpu-com-stress-ng-debian9/) e [avançado_02](https://www.cyberciti.biz/faq/stress-test-linux-unix-server-with-stress-ng/) ao stress-ng.




