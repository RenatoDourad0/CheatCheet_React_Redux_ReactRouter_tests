# SQL
  - MySQL é um SGBD - sistema de gerenciamento de banco de dados - ou seja, um serviço executado em segundo plano que auxilia no gerenciamento de db's
    - `cmd + b` para formatação automática 

## comandos do homebrew (uso local)
  - o servidor SQL precisa ser inicializado
    -  para inicializar o servidor `brew services run mysql`
    -  para parar o servidor `brew services stop mysql`
    -  para ativar a inicialização automatica junto com o computador `brew services start mysql`
    -  também disponivel nas configurações do mac

## conectando ao servidor
  - `mysql -u <nome-usuario> -p` aonde a flag `-u` permite definir o usuário (normalmete root) e a flag `-p` permite entrar com a senha do usuário
  - senha servidor mysql local '1234'

## portas e variaveis de ambiente
  - a porta padrão utilizada pelo mysql é a `3306`
  - `MYSQL_ROOT_PASSWORD=<senha>`

## Mysql conteinerizado
  - ` docker run --name <nome> -v /Users/renatocampos/mysql/<nome-pasta>:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=<senha> -d mysql:<tag>`
  - `docker exec -it <id_container> bash`
  - `mysql -u root -p`

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
  - `CONCAT` une colunas da tabela enquerida em uma so coluna
  - `DISTINCT` não seleciona dados repetidos
  - `COUNT ()` para totalizar valores não nulos de colunas da tabela
  - `LIMIT` limitar quantidade de resultados retornados
  - `OFFSET` junto com o LIMIT permite pular linhas iniciais da tabela
  - `ORDER BY <nome-coluna> ASC ou DESC, <nome-coluna-2> ASC ou DESC` para ordenar o resultado da query de forma crescente ou decrescente
  - `WHERE` permite fazer uma seleção
  - `LIKE` comparação por aproximação
    - coringa `%` representa um conjunto de caracteres
    - coringa `_` representa um caractere
  - `IN ()` permite definir um conjunto de valores a serem comparados no filtro
  - `BETWEEN <valor1> AND <valor2>` permite definir uma área de variação de valores a serem comparados no filtro
  - `ROUND` E `FLOOR` PARA RREDONDAR `ROUND(<dado>, <numero-de-casas-decimais>) ou FLOOR(<dado>)`

### operadores
  - `=` IGUAL
  - `>` MAIOR QUE
  - `<` MENOR QUE
  - `>=` MAIOR QUE OU IGUAL
  - `<=` MENOR QUE OU IGUAL
  - `<>` DIFERENTE DE
  - `AND` OPERADOR LÓGICO E
  - `OR` OPERADOR LÓGICO OU
  - `NOT` NEGAÇÃO
  - `IS` COMPARA COM VALORES BOOLEANOS (TRUE, FALSE, NULL)

### tipos de dados 
  - number (integer e float)
  - string
  - bool `0 / 1 - FALSE / TRUE`
  - date `'AAAA-MM-DD HH:MM:SS'` (date, dateTime, timeStamp)
    - [referência](https://www.w3resource.com/mysql/date-and-time-functions/date-and-time-functions.php) 
    - funções de transformação de data - ``` DATE(), YEAR(), MONTH(), DAY(), HOUR(), MINUTE(), SECOND() ```
    - funções de data - ```now(), curtime(), curday()```

### cast
  - conversão de tipos

### INSERT
  - adicionar dados a tabela
  - observar colunas com valores setadors automaticamente, essas não devem ser citadas no INSERT
  - INSERT IGNORE para adicionar dados ignorando possíveis erros, como informações duplicadas. A parte que funciona da query é executada independente de outra parte falhar
```sql
-- inserindo valores brutos
INSERT INTO nome_da_tabela (coluna1, coluna2) VALUES
('valor_1','valor_2'),
('valor_3','valor_4'),
('valor_5','valor_6');

-- inserindo valores dinâmicos (de outras tabelas)
INSERT INTO tabelaA (coluna1, coluna2)
    SELECT coluna1, coluna2
    FROM tabelaB
    WHERE nome_da_coluna <> 'algumValor'
    ORDER BY coluna_de_ordenacao;
```

### UPDATE
  - atualizar dados
  - para desabilitar o safe updates mode (permite updates somente se especificado o id da linha) executar a query `SET SQL_SAFE_UPDATES = 0;`
  - para habilitar o safe updates `SET sql_safe_updates=1, sql_select_limit=1000, max_join_size=1000000;`
```sql
-- update de uma coluna em uma linha
UPDATE nome_da_tabela
SET propriedade_a_ser_alterada = 'novo valor para coluna'
WHERE alguma_condicao -- importantíssimo aplicar o WHERE para não alterar a tabela inteira!
[ORDER BY expressao [ ASC | DESC ]]
[LIMIT quantidade_resultados];

-- update de varias colunas em uma linha
UPDATE nome_da_tabela
SET propriedade_a_ser_alterada = 'novo valor para coluna', propriedade_a_ser_alterada_2 = 'novo valor para coluna'
WHERE alguma_condicao;

-- update de uma coluna em varias linhas
UPDATE nome_da_tabela
SET propriedade_a_ser_alterada = 'novo valor para coluna'
WHERE propriedade IN (conjunto);
-- ou
UPDATE nome_da_tabela
SET propriedade_a_ser_alterada = (
CASE propriedade WHEN valor THEN 'novo valor' -- se propriedade = valor, alterar propriedade_a_ser_alterada para 'novo valor'
              WHEN valor2 THEN 'outro valor'
              WHEN valor3 THEN 'outro valor' 
	      ELSE propriedade_a_ser_alterada -- em todos os outros casos, mantém-se o valor existente
END);
-- ou
UPDATE nome_da_tabela
SET propriedade_a_ser_alterada = (
CASE
  WHEN condição THEN 'novo valor' -- se condição (propriedade = valor), alterar propriedade_a_ser_alterada para 'novo valor'
  WHEN condição2 THEN 'outro valor'
  ELSE propriedade_a_ser_alterada -- em todos os outros casos, mantém-se o valor existente
END);
```

### DELETE
  - remover dados
  - caso a linha a ser excluida possua referencias externas que utilizam de restrições ON DELETE (NO ACTION, RESTRICT, SET NULL, CASCADE) o comando pode ser bloqueado. Para resolver deve-se primeiro excluir as informações da tabela extrangeira
```sql
DELETE FROM banco_de_dados.tabela
WHERE coluna = 'valor';
-- O WHERE é opcional. Porém, sem ele, todas as linhas da tabela seriam excluídas.
```

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
