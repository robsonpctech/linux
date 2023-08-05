## Se a tela estiver bloqueada ele fará o logout do usuário.
> script que verifica o status da tela bloqueada usando o comando xdg-screensaver e se a tela estiver bloqueada, ele fará o logout do usuário

Cria uma pasta para guardar a automação de deslogar usuário após 2h com a tela de bloqueia ativada.
```
mkdir -p /usr/local/bin/automacoes
```

Dentro de cd /usr/local/bin/automacoes/ crie o arquivo executável

```
nano desloga_user_ativ_maisde2h.sh

```

Cole o seguinte conteúdo no arquivo:

```
#!/bin/bash

# Verifica se a tela está bloqueada
if xdg-screensaver status | grep -q "is active"; then
    echo "Tela bloqueada. Desconectando usuário..."
    pkill -KILL -u $USER
else
    echo "Tela não bloqueada."
fi

```

Da permissão de execução ao arquivo

```
chmod +x /usr/local/bin/automacoes/desloga_user_ativ_maisde2h.sh

```
Agende a execução do script com o cron:
Agende a execução do script a cada período de tempo

```
crontab -e

```
Adicione a seguinte linha ao arquivo para agendar a execução a cada 2h

```

0 */2 * * * /usr/local/bin/automacoes/desloga_user_ativ_maisde2h.sh

```
