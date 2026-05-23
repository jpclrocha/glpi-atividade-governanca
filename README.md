# Repositorio criado para o trabalho de Governança de TI

Equipe: João Laranjeira, Maria Eduarda, Davi da Silva e Daniel do Carmo

Esse repositorio contem o docker compose disponivel em: https://hub.docker.com/r/glpi/glpi#how-to-use-this-image

Antes de dar `docker compose up -d, é necessário criar um arquivo .env`, com as variaveis de ambientes reais a serem utilizadas. O repositorio contem um .env.example, com todas as variaveis necessarias, e valores simples para cada uma delas

Atenção, para os proximos passos precisamos identificar os ids dos containers do GLPI e do MySQL, rodamos o comando:

`docker ps`

Para localizar o id do container do mysql, por padrao, no docker compose deixei o container com o nome de `glpi_db`, entao eh so procurar por ele e achar o id. O mesmo segue para o GLPI, que deixei nomeado como glpi

Para configurar o suporte das timezones no GLPI, precisamos dar GRANT para o usuario `glpi (aqui vai depender de como voce configurou seu .env)` na tabela mysql.time_zone. Com o container do docker rodando, execute o seguinte comando no terminal:

Aqui vamos usar o id do container do MySQL
docker exec -it <db_container_id> mysql -u root -p -e "GRANT SELECT ON mysql.time_zone_name TO '<usuario do banco configurado no .env>'@'%';FLUSH PRIVILEGES;"

Depois disso, é só rodar:

Aqui vamos usar o id do container do GLPI
docker exec -it <glpi_container_id> /var/www/glpi/bin/console database:enable_timezones
