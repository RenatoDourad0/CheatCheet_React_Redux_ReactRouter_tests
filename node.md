# node.js

## leitura e esqcrita de arquivos
  -  terceiro parâmentro flag opcional
      - `w` para escrita
      - `wx` lança errro caso o arquivo exista
  - escrita
```js
 const fs = require('fs').promises;
 
 async function main() {
  try {
    await fs.writeFile('./meu-arquivo.txt', 'Meu textão');
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

async function main() {
  try {
    const data = await fs.readFile('./meu-arquivo.txt', 'utf-8');
    console.log(data);
  } catch (err) {
    console.error(`Erro ao ler o arquivo: ${err.message}`);
  }
}

main()
```
