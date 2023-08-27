# Criando um Servidor DNS Local com Docker

Este guia mostra como configurar um servidor DNS local usando Docker.

## Passo 1: Instalar o Docker

Certifique-se de ter o Docker instalado no seu sistema. Você pode seguir as instruções de instalação no site oficial do Docker, de acordo com o seu sistema operacional: [Docker Installation Guide](https://docs.docker.com/get-docker/)

## Passo 2: Criar um arquivo de configuração para o servidor DNS

Crie um arquivo chamado `named.conf` para a configuração do servidor DNS:

```plaintext
// named.conf
options {
    directory "/var/named";
    allow-query { localhost; };
    recursion yes;
    listen-on port 53 { any; };
};

zone "local" IN {
    type master;
    file "local.zone";
};
```

## Passo 3: Criar uma zona DNS

Crie um arquivo chamado local.zone na mesma pasta que o arquivo named.conf. Este arquivo contém as entradas DNS para a zona "local". Aqui está um exemplo simples:

```plaintext
; local.zone
$TTL 86400
@   IN SOA  ns.local. root.local. (
    2010010901  ; Serial
    3600        ; Refresh
    1800        ; Retry
    604800      ; Expire
    86400       ; Minimum TTL
)
@   IN NS   ns.local.
@   IN A    127.0.0.1
```

## Passo 4: Iniciar um contêiner DNS usando o Docker

Agora, você pode usar uma imagem Docker para configurar o servidor DNS. Uma opção é usar a imagem oficial bind:

```plaintext
docker run -d --name dns-server -v /caminho/para/os/seus/arquivos/config:/etc/bind -p 53:53/udp --restart=always --hostname ns.local bind:latest
```

Substitua /caminho/para/os/seus/arquivos/config pelo caminho absoluto para a pasta onde você colocou os arquivos de configuração.

## Passo 5: Testar o servidor DNS local

Agora que seu servidor DNS local está em execução, você pode testá-lo usando a ferramenta dig ou nslookup no seu terminal. Por exemplo:

```plaintext
dig @127.0.0.1 example.local
```

## Adicionar Apontamentos
Para adicionar apontamentos, edite o arquivo local.zone e adicione entradas como esta:

```plaintext
; local.zone
$TTL 86400
@   IN SOA  ns.local. root.local. (
    2010010901  ; Serial
    3600        ; Refresh
    1800        ; Retry
    604800      ; Expire
    86400       ; Minimum TTL
)
@   IN NS   ns.local.
@   IN A    127.0.0.1
admin   IN A    192.188.0.1
```
Após editar o arquivo, reinicie o contêiner DNS:

bash
```
docker restart dns-server
```
Testar Apontamento
Teste o apontamento usando dig para o nome de host "admin.com":

```
dig @127.0.0.1 admin.com
```

Lembre-se de substituir `/caminho/para/os/seus/arquivos/config` pelo caminho real e ajustar as configurações conforme necessário. Agora você pode salvar esse conteúdo em um arquivo `.md` e fazer o upload para o GitHub. Certifique-se de também incluir os detalhes específicos do seu sistema e configuração, conforme necessário.
