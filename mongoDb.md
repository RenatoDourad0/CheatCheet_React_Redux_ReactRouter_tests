# MONGODB

## uso com docker
- iniciando o container: `docker run --name <nome-do-container> -d -p 27017:27017 mongo:<tag>`
  - mapeamento de portas para usar softwares locais(mongo for vscode) no container   
- acessando o container: `docker exec -it <nome-do-container-ou-id> mongo` ou `mongosh`
- importação de arquivos locais com mongoimport:
  - criar o arquivo: `docker cp nome-do-arquivo.json <nome-do-container-ou-id>:/tmp/nome-do-arquivo.json`
  - importar: `docker exec <nome-do-container-ou-id> mongoimport -d <nome-do-banco> -c <nome-da-coleção> --file /tmp/nome-do-arquivo.json`
  - flag `--jsonArray` permite importar arquivos json que estão organizados como matriz em um único arquivo (array de objetos)

## definições
- A estrutura de armazenamento do MongoDB consiste em:
  - ter diversos bancos de dados;
  - dentro destes bancos temos as coleções (que seriam equivalentes às tabelas dos bancos de dados relacionais);
  - dentro destas coleções temos os documentos (que seriam equivalentes aos registros dos bancos de dados relacionais).

## métodos
- use
  - definir o banco de dados a ser usado. Ficará acessivel na variavel `db`
- createCollection
  - criar uma nova coleção no banco 
- insertOne
  - o atributo `_id` é gerado automaticamente. Caso desejar substituir deve ser declarado na query com o valor desejado  
- insertOne
- insertMany
  - insere um array de documentos 
  - opções
    - ordered: boleano que define se as incerções serão ordenadas (para as inserções em caso de erro em um documento) ou não ordenadas (insere todos os documentos, pulando eventuais erros)
- find
  - opções
    - query: objeto com os parâmentros a serem buscados. `{}` para retornar todos os documentos
    - projections: objeto opcional que permite selecionar os parametros desejadfos no retorno
      - os atribulos podem ter o valor 1 e 0. Sendo 1 um atributo a ser incluido e 0 a ser excluido. Atributos omitidos serão excluidos, exceto o `_id` que deve ser explicitada sua exclusão
```mongodb
db.movies.findOne(
    { "title" : "Forrest Gump", "information.awarded": true  },
    { "title" : 1, "imdb_rating" : 1, "_id": 0 }
)
```
- limit
  - `db.collection.find(<query>).limit(<número>)` 
- skip
  - permite pular documentos antes de começar a executar a query. Recebe um numero. ex: `db.bios.find().skip(2)`
- sort
- update
- countDocuments
  - retorna o número de documentos de uma coleção, e também pode receber um critério de seleção para retornar apenas o número de documentos que atendam a esse critério  
- pretty
  - retorna os documentos com identação e quebra de linha (mais útil no shell) 

## operadores
- sintaxe
  - `{ <campo>: { <operador>: <valor> } }` 
- comparação
  - deve se ter atenção aos tipos dos dados na query de comparação e nos documentos sendo buscados pois dada a natureza não relacional do nosql documentos diferentes podem ter tipos diferentes para um mesmo atributo
  - $gt: maior que (greater than)
  - $gte: maior ou igual a
  - $lt: menor que (less than)
  - $lte: menor ou igual a
  - $eq: igual a
  - $ne: diferente de
  - $in: pertence a um grupo (array)
  - $nin: não pertence a um grupo
- lógicos
  - $and
  - $or
- $not
- $nor
- $exists
- $all
- $elemMatch
- $size
- $expr
- $regex
- $mod
- $set
- $mul
- $inc
- $min e $max
- $currentDate
- $unset
- $push
- $pop
- $pull
- $addToSet
