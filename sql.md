# SQL

## comandos do homebrew
  - o servidor SQL é um servço que precisa ser inicializado
    -  para inicializar o servidor `brew services run mysql`
    -  para ativar a inicialização automatica junto com o computador `brew services start mysql`
    -  para desativar a inicialização automatica junto com o computador `brew services stop mysql`

## comandos de manipulação
  - `USE <nome-banco>;` para acessar uma db
  - `SELECT * FROM <nome-banco>.<nome-tabela>;` outra forma de acessar a db já especificando a tabela (mais direta)
  - `SHOW TABLES;` para mostrar o nome das tabelas da db
  - `DESCRIBE <nome-tabela>;` para mostrar uma tabela
  - `CREATE DATABASE <nome-banco>;` para criar uma db
