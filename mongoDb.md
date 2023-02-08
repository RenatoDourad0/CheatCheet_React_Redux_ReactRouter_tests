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
- dropDatabase
  - apagar um banco 
- insertOne
  - o atributo `_id` é gerado automaticamente. Caso desejar substituir deve ser declarado na query com o valor desejado  
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
  - `db.example.find().sort({ <atributo>: <valor (1 ou -1)> })`
  - deve ser usado no retorno de um resultadode busca
  - atributos com valor positivo (1) iram geram uma ordenação em ordem crescente ou alfabética. Atributos com valor negativo (-1) fazem o contrário
  
- updateOne
  - altera apenas o primeiro documento que satisfaça o critério de filtro
```
db.inventory.updateOne(  // atualiza o primeiro elemento com o campo item igual a paper
  { item: "paper" },     // e atualiza os valores das propriedades size.uom e status
  { $set: { "size.uom": "cm", status: "P" } }
);
```
- updateMany
  - altera todos os documentos que satisfaçam o critério de filtro
- deleteOne
  - remove o primeiro documento que satisfaça a query
  
- deleteMany
  - remove todos os documentos que satisfação a query 
  
- countDocuments
  - retorna o número de documentos de uma coleção, e também pode receber um critério de seleção para retornar apenas o número de documentos que atendam a esse critério  
  
- pretty
  - retorna os documentos com identação e quebra de linha (mais útil no shell) 

## operadores
#### sintaxe básica
  - `{ <campo>: { <operador>: <valor> } }`
  
#### operadores de comparação simples
- importante: deve se ter atenção aos tipos dos dados na query de comparação e nos documentos sendo buscados pois dada a natureza não relacional do nosql documentos diferentes podem ter tipos diferentes para um mesmo atributo
- $gt
  - maior que (greater than)
- $gte
  - maior ou igual a
- $lt
  - menor que (less than)
- $lte
  - menor ou igual a
- $eq
  - igual a
- $ne
  - diferente de
- $in
  - pertence a um grupo (array)
- $nin
  - não pertence a um grupo

#### operadores lógicos
- $and
  - retorna os documentos nos quais todas as expressoões sejam verdadeiras 
  - `{ $and: [{ <expression1> }, { <expression2> }, ... , { <expressionN> }] }`
- $or
  - retorna os documentos nos quais alguma das expressoões sejam verdadeiras 
  - `{ $or: [{ <expression1> }, { <expression2> }, ... , { <expressionN> }] }`
- $not
  - retorna os documentos nos quais a expressão seja falsa
  - `{ campo: { $not: { <operador ou expressão> } } }`
- $nor
  - retorna os documentos em que todas as expressões do array sejam falsas
  - `{ $nor: [ { <expressão1> }, { <expressão2> }, ...  { <expressãoN> } ] }`
- $exists
  - recebe um valor boleano e retorna os documentos que contenham o atributo em caso de verdadeiro ou os documentos que não contem o atributo em caso de falso     
 
#### operadores de consulta para arrays
- $all
  - seleciona todos os documentos em que o valor do campo é um array que contenha todos os elementos especificados, independente de sua ordem ou da existencia de outros elementos além dos especificados
  - similar ao and porém para buscar em arrays
  - `db.inventory.find({ tags: { $all: ["red", "blank"] } });`
- $elemMatch
  - seleciona os documentos que contêm um campo do tipo array com pelo menos um elemento que satisfaça todos os critérios de seleção especificados
  - `{ campo: { $elemMatch: { $gte: 80, $lt: 85 } } }`
- $size
  - seleciona documentos em que um array contenha um número de elementos especificado  
  - `{ tags: { $size: 2 }`

####  operadores de consulta
- $expr
  - permite que você utilize expressões de agregação e construa queries que comparem campos no mesmo documento 
  - `{ $expr: { $gt: [ "$spent", "$budget" ] }}` 
  - o $ deve ser utilizado para indicar que a string entre aspas referencia um campo
- $regex
  - fornece os “poderes” das expressões regulares para seleção de strings  
  - `{ sku: { $regex: /789$/ } }`
- $mod
  - seleciona todos os documentos em que o valor do campo dividido por um divisor seja igual ao valor especificado (executa a operação matemática módulo)
  - `{ qty: { $mod: [<divisor>, <resultado>] } }` ex: `{$mod : [4,0]}` :. campo / 4 = 0

#### operadores de atualização
- $set
  - altera o valor ou cria um campo específico
  - ` { $set: { "details.make": "zzz" } }`
- $mul
  - multiplica o valor de um campo por um número especificado, persistindo o resultado dessa operação sem a necessidade do operador $set. Casoo campos especificado não exista será criado com o valor zero do mesmo tipo do multiplicador
  - `{ $mul: { price: NumberDecimal("1.25"), qty: 2 } }` :. o valor original de price é multiplicado por 1.25 e o de qty por 2
- $inc
  - incrementar ou decrementar valores em um campo específico. Permite fazer operações sem antes buscar pelos valores antigos do campo 
  - `{ $inc: { quantity: -2, "metrics.orders": 1 } }`   
- $min
  - Altera o valor do campo atual para o valor passado pelo método se o valor passado pelo método for menor do que o valor do campo atual
  - o mesmo operador limita a busca e altera os valores do campo
- $max
  - Altera o valor do campo atual para o valor passado pelo método se o valor passado pelo método for maior do que o valor do campo atual
  - `{ $max: { campo: 75 } }`
- $currentDate
  - atribui ao valor de um campo a data corrente, utilizando um tipo Date ou timestamp. Se você não especificar o tipo, por padrão, o MongoDB atribui o valor do tipo Date 
  - a especificação do tipo pode ser um valor boleano (true para tipo Date) ou um objeto com a chave $type de valor 'date'ou 'timestamp'
  - `{ $currentDate: { <campo>: <typeSpecification>, ... } }`
```
{ $currentDate: {
      lastModified: true,
      "cancellation.date": { $type: "timestamp" }
    }, $set: {
      "cancellation.reason": "user request",
      status: "D"
    }
  }
```
- $rename
  - recebe um documento contendo o nome atual do campo e o novo nome. Pode ser utilizado com os métodos updateOne() ou updateMany(), e também pode receber um critério de seleção de documentos
  - `{ $rename: {<nome do campos antigo>: "novo nome"}}` 
- $unset
  - remove um ou mais campos de um documento
  - `{ $unset: { <campo>: "" }`
#### operadores de atualização de arrays
- $push
- $pop
- $pull
- $addToSet
