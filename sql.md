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


## comandos de definição
  - `CREATE` Para criar bancos de dados, tabelas, índices, views, procedures, functions e triggers;
  - `ALTER` Para alterar a estrutura de qualquer objeto;
  - `DROP` Permite deletar objetos;
  - `TRUNCATE` Apenas esvazia os dados dentro de uma tabela, mas a mantém no banco de dados.
  
## comandos de manipulação
  - `USE <nome-banco>;` para acessar uma db
  - `SHOW TABLES;` para mostrar o nome das tabelas da db
    - `SELECT * FROM <nome-banco>.<nome-tabela>;` outra forma de acessar a db já especificando a tabela (mais direta) 
  - `DESCRIBE <nome-tabela>;` para mostrar uma tabela
  - `SELECT <informação> AS <nome-coluna> FROM <DB.tabela>` Usado para buscar dados em um banco de dados;
  - `INSERT` Insere dados em uma tabela;
  - `UPDATE` Altera dados dentro de uma tabela;
  - `DELETE` Exclui dados de uma tabela.
  - `CONCAT` une colunas da tabela enquerida em uma so coluna
  - `DISTINCT` não seleciona dados repetidos
  - `COUNT()` para totalizar valores não nulos de colunas da tabela
  - `LIMIT` limitar quantidade de resultados retornados
  - `OFFSET` junto com o LIMIT permite pular linhas iniciais da tabela
  - `ORDER BY <nome-coluna> ASC ou DESC, <nome-coluna-2> ASC ou DESC` para ordenar o resultado da query de forma crescente ou decrescente
  - `WHERE` permite fazer uma seleção
    - usado com operadores aritimeticos( =, >, <, !=) 
    - com NOT para relação de diferente
    - com LIKE para relação de proximidade (por exemplo uma string parcial '%...')
  - `ROUND` E `FLOOR` PARA RREDONDAR `ROUND(<dado>, <numero-de-casas-decimais>) ou FLOOR(<dado>)`

## comandos de controle
  - `GRANT` Concede acesso a um usuário;
  - `REVOKE` Remove acessos concedidos através do comando GRANT.

## comandos transacionais 
  - `COMMIT` Muda suas alterações de temporárias para permanentes no seu banco de dados;
  - `ROLLBACK` Desfaz todo o impacto realizado por um comando;
  - `SAVEPOINT` Define pontos para os quais uma transação pode voltar. É uma maneira de voltar para pontos específicos de sua query;
  - `TRANSACTION` Comandos que definem onde, como e em que escopo suas transações são executadas.

## cheat sheet
![IMG_6192](https://user-images.githubusercontent.com/99191410/196166224-f812742b-3297-4c6b-8771-c7cde7b6fc4d.jpeg)  
![IMG_6193](https://user-images.githubusercontent.com/99191410/196166430-a1896024-1cae-4c63-b175-c6d34ea3d10d.jpeg)
