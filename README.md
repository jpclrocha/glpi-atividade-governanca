# Governança de TI — GLPI com Docker

Repositório criado para o trabalho de **Governança de TI**.

## Equipe

- João Laranjeira
- Maria Eduarda
- Davi da Silva
- Daniel do Carmo

## Sobre o projeto

Este repositório contém um `docker-compose.yml` baseado na imagem oficial do GLPI disponível no Docker Hub:

https://hub.docker.com/r/glpi/glpi#how-to-use-this-image

## Configuração inicial

Antes de executar o Docker Compose, é necessário criar um arquivo `.env` com as variáveis de ambiente reais que serão utilizadas.

O repositório contém um arquivo `.env.example` com todas as variáveis necessárias e valores simples de exemplo.

Crie o arquivo `.env` com base no exemplo:

```bash
cp .env.example .env
```

Depois, edite o arquivo `.env` conforme necessário.

## Subindo os containers

Após configurar o arquivo `.env`, execute:

```bash
docker compose up -d
```

## Identificando os containers

Para os próximos passos, será necessário identificar os IDs dos containers do GLPI e do MySQL.

Execute:

```bash
docker ps
```

Por padrão, no `docker-compose.yml`:

- O container do MySQL foi nomeado como `glpi_db`
- O container do GLPI foi nomeado como `glpi`

Localize esses containers na saída do comando `docker ps` e copie seus respectivos IDs.

## Configurando suporte a timezones no GLPI

Para configurar o suporte a timezones no GLPI, é necessário conceder permissão ao usuário do banco de dados na tabela `mysql.time_zone_name`.

> O usuário do banco depende do valor configurado no arquivo `.env`.

Com os containers rodando, execute o comando abaixo usando o ID do container do MySQL:

```bash
docker exec -it <db_container_id> mysql -u root -p -e "GRANT SELECT ON mysql.time_zone_name TO '<usuario_do_banco_configurado_no_env>'@'%'; FLUSH PRIVILEGES;"
```

Substitua:

- `<db_container_id>` pelo ID do container MySQL
- `<usuario_do_banco_configurado_no_env>` pelo usuário configurado no `.env`

## Habilitando timezones no GLPI

Depois de conceder a permissão no banco, execute o comando abaixo usando o ID do container do GLPI:

```bash
docker exec -it <glpi_container_id> /var/www/glpi/bin/console database:enable_timezones
```

Substitua:

- `<glpi_container_id>` pelo ID do container GLPI

## Resumo dos comandos principais

```bash
cp .env.example .env
docker compose up -d
docker ps
```

Comando para configurar permissão no MySQL:

```bash
docker exec -it <db_container_id> mysql -u root -p -e "GRANT SELECT ON mysql.time_zone_name TO '<usuario_do_banco_configurado_no_env>'@'%'; FLUSH PRIVILEGES;"
```

Comando para habilitar timezones no GLPI:

```bash
docker exec -it <glpi_container_id> /var/www/glpi/bin/console database:enable_timezones
```
