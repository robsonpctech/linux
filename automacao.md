## Se a tela estiver bloqueada ele fará o logout do usuário
> script que verifica o status da tela bloqueada usando o comando xdg-screensaver e se a tela estiver bloqueada, ele fará o logout do usuário

Cria uma pasta para guardar a automação de deslogar usuário após 2h com a tela de bloqueio ativada.
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

Da permissão de execução ao arquivo.

```
chmod +x /usr/local/bin/automacoes/desloga_user_ativ_maisde2h.sh
```

Agende a execução do script com o cron, para cada período de tempo

```
crontab -e
```

Adicione a seguinte linha ao arquivo para agendar a execução a cada 2h

```
0 */2 * * * /usr/local/bin/automacoes/desloga_user_ativ_maisde2h.sh
```
## Apagar o conteúdo de /home exceto a pasta lost+found e a pasta xyz
> script que todos os dias às 21h irá apagar apagar

Crie um novo arquivo de script, dentro de cd /usr/local/bin/automacoes/ crie o arquivo executável

```
nano limpar_home.sh
```
Insira o seguinte conteúdo no arquivo limpar_home.sh:
```
#!/bin/bash

# Define o caminho da pasta /home
pasta_home="/home"

# Lista de pastas que você deseja manter
pastas_a_manter=("lost+found" "fadmin")

# Loop para percorrer todas as pastas na pasta /home
for item in "$pasta_home"/*; do
    if [ -d "$item" ]; then
        nome_da_pasta=$(basename "$item")
        if [[ ! " ${pastas_a_manter[@]} " =~ " ${nome_da_pasta} " ]]; then
            echo "Removendo conteúdo de: $nome_da_pasta"
            rm -rf "$item"/*
        fi
    fi
done
```

Torne o script executável usando o seguinte comando:
```
chmod +x limpar_home.sh
```

Agende a execução diária do script usando o Cron
```
crontab -e
```

Adicione a seguinte linha ao arquivo do Cron para agendar a execução diária do script às 21h:
```
0 21 * * * /caminho/completo/para/limpar_home.sh
```
