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
- insertOne
- insertMany
- find
- sort
- update

## operadores
- comparação
  - 
- lógicos
  -   
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
