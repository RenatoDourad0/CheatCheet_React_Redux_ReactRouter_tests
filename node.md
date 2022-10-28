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
