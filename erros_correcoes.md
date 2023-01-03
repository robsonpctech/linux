### Problema 01
>E: Release file for http://br.archive.ubuntu.com/ubuntu/dists/jammy-updates/InRelease is not valid yet (invalid for another 2h 8min 19s). Updates for this repository will not be applied

**Solução**:
```
sudo rm /var/lib/apt/lists/*
sudo apt update
```
Se persistir o erro, repita os comandos acima e troque os endereços em:
```
/etc/apt/sources.list
```
### Problema 02
>W: Tried to start delayed item http://in.archive.ubuntu.com/ubuntu focal-security i386 Contents (deb), but failed
Err:1 https://download.docker.com/linux/ubuntu jammy InRelease
Splitting up /var/lib/apt/lists/download.docker.com_linux_ubuntu_dists_jammy_InRelease into data and signature failed

**Solução**:

No ambiente gráfico selecione o app:
- Software & Updates: Desative todos os ppa e as atualizações, procure e selecione o melhor servidor. Atualize.

### Problema 03
>W: http://ppa.launchpad.net/ondrej/php/ubuntu/dists/jammy/InRelease: Key is stored in legacy trusted.gpg keyring (/etc/apt/trusted.gpg), see the DEPRECATION section in apt-key(8) for details.

**Solução**:
```
apt-key list
```
Irá aparecer uma mensagem como a descrita abaixo:
```
pub   rsa1024 2010-12-29 [SC]
      36E8 1C92 67FD 1383 FCC4  4909 83FB A175 1378 B444
uid  [ desconhecida] Launchpad PPA for LibreOffice Packaging
```
Onde no exemplo tem 1378 B444, altere o número na frente do export abaixo e mude o nome do programa a frente.
```
sudo apt-key export 1378B444 | sudo gpg --dearmour -o /etc/apt/trusted.gpg.d/team-libreoffice.gpg
```
em seguida atualize:

```
apt update e apt upgrade
```
<h3>Problema 04 </h3>

>usuario tal não está no arquivo sudoers. Este incidente será relatado.

**Solução**:

Edite: /etc/sudoers.d e crie um arquivo chamado ldap

coloque as linhas abaixo:

```
%devel ALL=(ALL) ALL
%domain_admin ALL=(ALL) ALL
```
dessa forma caso o sistema operacional atualize ele não vai perder as configurações de ldap, sudo etc.

<h3>Problema 05 </h3>

>you are in emergency mode after logging in type journalctl -xb

>BusyBox v1.30.1 built-in shell (ash)
>Enter ‘help’ for a list of built-in commands.
>(initramfs):

**Solução**:
```
blkid
e2fsck -yfr /dev/mapper/particao desejada
reboot
```
<h3>Problema 06 </h3>

> Problemas ao reproduzir som

Plano A
```
inxi -SMA
alsa force-reload
```
Plano B
```
apt-get install –reinstall alsa-base pulseaudio
alsa force-reload
```
Plano C
```
mv ~/.config/pulse ~/.config/old_pulse
```

Plano D
```
alsamixer
alsamixer -c 1
```
Plano E
```
gedit /etc/default/speech-dispatcher
Aqui, altere RUN=yes para RUN=no . Reinicie e aproveite o som
```
Plano F
```
gedit /etc/modprobe.d/alsa-base.conf
```
Adicione a seguinte linha ao final deste arquivo:
```
options snd-hda-intel dmic_detect=0
```






