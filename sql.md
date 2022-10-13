# SQL
  - MySQL é um SGBD - sistema de gerenciamento de banco de dados - ou seja, um serviço executado em segundo plano que auxilia no gerenciamento de db's

## comandos do homebrew (uso local)
  - o servidor SQL precisa ser inicializado
    -  para inicializar o servidor `brew services run mysql`
    -  para parar o servidor `brew services stop mysql`
    -  para ativar a inicialização automatica junto com o computador `brew services start mysql`
    -  também disponivel nas configurações do mac

## conectando ao servidor
  - `mysql -u <nome-usuario> -p` aonde a flag `-u` permite definir o usuário (normalmete root) e a flag `-p` permite entrar com a senha do usuário

## portas e variaveis de ambiente
  - a porta padrão utilizada pelo mysql é a `3306`
  - `MYSQL_ROOT_PASSWORD=<senha>`

## sql conteinerizado
  -

## comandos de manipulação
  - `USE <nome-banco>;` para acessar uma db
  - `SELECT * FROM <nome-banco>.<nome-tabela>;` outra forma de acessar a db já especificando a tabela (mais direta)
  - `SHOW TABLES;` para mostrar o nome das tabelas da db
  - `DESCRIBE <nome-tabela>;` para mostrar uma tabela
  - `CREATE DATABASE <nome-banco>;` para criar uma db
