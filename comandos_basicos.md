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
rsync -varhzP origem fadmin@ip de destino:/destino
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