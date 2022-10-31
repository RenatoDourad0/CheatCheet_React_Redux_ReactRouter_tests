# node.js

## leitura e escrita de arquivos com fs
  -  terceiro parâmentro flag opcional
      - `w` para escrita
      - `wx` lança errro caso o arquivo exista
  - escrita
```js
 const fs = require('fs').promises;
 const path = require('path');
 
 async function main() {
  try {
    await fs.writeFile(path.resolve(__dirname, './meu-arquivo.txt'), 'Meu textão');
    console.log('Arquivo escrito com sucesso!');
  } catch (err) {
    console.error(`Erro ao escrever o arquivo: ${err.message}`);
  }
}

main()
 ```
  - leitura
```js
const fs = require('fs').promises;
const path = require('path');

async function main() {
  try {
    const data = await fs.readFile(path.resolve(__dirname, './meu-arquivo.txt'), 'utf-8');
    console.log(data);
  } catch (err) {
    console.error(`Erro ao ler o arquivo: ${err.message}`);
  }
}

main()
```

## express
- ambiente
  - `npm init -y` 
  - `npm i express`
  - `npm i nodemon -D`
  - `npm init @eslint/config` - verificar plugins e regras no arquivo .eslintrc.json - [docs](https://eslint.org/docs/latest/user-guide/configuring/configuration-files)
    - eslint para trybe: 
      - `npm i eslint@6.8 eslint-config-trybe-backend@1.0 -D` 
      - ```
        // eslintrc.json
        {
        "env": {
          "es2020": true
        },
        "extends": "trybe-backend",
        "rules": {
          "sonarjs/no-duplicate-string": ["error", 5]
        }
        }
        ```
  - `touch .eslintignore` - node_modules, ./*.config.js
  - `git init`
  - `touch .gitignore` - node_modules, .env
  - adicionar ao package.json
```js
"main": "src/server.js"
"scripts": {
"start": "node src/server.js",
"dev": "nodemon src/server.js",
"lint": "eslint --no-inline-config --no-error-on-unmatched-pattern -c .eslintrc.json .",
"lint:fix":"eslint --fix --ext .js,.jsx ." // adicionar extenções desejadas
},
```

- inicializando
```js
// src/app.js
const express = require('express');

const app = express();

module.exports = app;

// src/server.js
const app = require('./app');

app.listen(3001, () => console.log('server running on port 3001'));
```

- sintaxe rotas
  - `'/ab?cd'` o caractere anterior ao `?` (b) é opcional
  - `'/ab+cd'` o caractere anterior ao `+` (b) pode se repetir
  - `'/ab*cd'` o `*` representa um conjunto de caracteres aleatórios
  - `'/a(bc)?de'` o `( )` agrupa caracteres

- requisição GET
```js
app.get('/', (req, res, next) => {
  try {
  res.status(200).json({ message: 'Olá Mundo!' }));
  next();
} catch(e) {
  res.status(500).json({message: e.message});
}}, (req, res) => {
...
})
```
  - parâmetro `res`
    - res.download()	Solicita que seja efetuado o download de um arquivo
    - res.end()	Termina o processo de resposta
    - res.json()	Envia uma resposta JSON
    - res.jsonp()	Envia uma resposta JSON com suporta ao JSONP
    - res.redirect()	Redireciona uma solicitação
    - res.render()	Renderiza um modelo de visualização
    - res.send()	Envia uma resposta de vários tipos
    - res.sendFile	Envia um arquivo como um fluxo de octeto
    - res.sendStatus()	Configura o código do status de resposta e envia a sua representação em sequência de caracteres como o corpo de resposta
  
  - requisição GET por query
    - `req.query` 
    - `/rota?variavel1=valor&variavel1=valor&variavelN=valor`
      - `/rota` é a rota a se buscar
      - `?` é o inicio da query
      - `variavel1=valor` são os valores a serem passados
      - `&` é o separador entre valores
      - dado disponivel na chave `query` sempre em formato de string

  - requisição GET por parametros
    - `req.params`
    - `/rota/:variavelX/:varivelZ` ex: /user/3345/active
      - `/rota` é a rota a se buscar
      - `/:` o indicador de que um parametro será passado
      - `variavelN` o valor a ser recebido
      - dado disponivel na chave `params` sempre em formato de string
