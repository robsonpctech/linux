## Problema 01
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
## Problema 02
>W: Tried to start delayed item http://in.archive.ubuntu.com/ubuntu focal-security i386 Contents (deb), but failed
Err:1 https://download.docker.com/linux/ubuntu jammy InRelease
Splitting up /var/lib/apt/lists/download.docker.com_linux_ubuntu_dists_jammy_InRelease into data and signature failed

**Solução**:

No ambiente gráfico selecione o app:
- Software & Updates: Desative todos os ppa e as atualizações, procure e selecione o melhor servidor. Atualize.

## Problema 03
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
## Problema 04

>usuario tal não está no arquivo sudoers. Este incidente será relatado.

**Solução**:

Edite: /etc/sudoers.d e crie um arquivo chamado ldap

coloque as linhas abaixo:

```
%devel ALL=(ALL) ALL
%domain_admin ALL=(ALL) ALL
```
dessa forma caso o sistema operacional atualize ele não vai perder as configurações de ldap, sudo etc.

## Problema 05

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
## Problema 06

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

## Problema 07

> Problema de autenticação no github

>remote: Support for password authentication was removed on August 13, 2021.

>remote: Please see https://docs.github.com/en/get-started/getting-started-with-git/about-remote-repositories#cloning-with-https-urls for information on currently recommended modes of authentication.

>fatal: Authentication failed for ' https://github.com/

**Solução**:
```
ssh-keygen -t ed25519 -C "seu e-mail do github"
ls -al ~/.ssh
exec ssh-agent bash
cat ~/.ssh/id_ed25519.pub
```
Copie a chave pública que estará abaixo do último comando e entre na url da sua conta no github e vá em:

```
settings ---> ssh and gpg keys ---> new ssh key = e cole a chave
```

em seguida vá em:

```
settings ---> Developer Settings ---> Tokens Classic ---> Generate New Token
```
Copie esse token e use ele como senha quando for fazer o git push

**Para salvar o Token no seu computador sem digitar novamente**

```
$ git config --global user.name "your_github_username"
$ git config --global user.email "your_github_email"
$ git config -l
```
Para testar faça um git push ou git clone

Agora armazene o registro fornecido em seu computador para lembrar o token:

```
$ git config --global credential.helper cache
```

Se necessário, a qualquer momento você pode excluir o registro de cache:

```
$ git config --global --unset credential.helper
$ git config --system --unset credential.helper
```

Agora tente puxar com -v para verificar

```
$ git pull -v
```

Linux/Debian (Clone da seguinte forma):

```
git clone https://<tokenhere>@github.com/<user>/<repo>.git
```