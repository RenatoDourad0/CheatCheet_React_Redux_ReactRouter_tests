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
