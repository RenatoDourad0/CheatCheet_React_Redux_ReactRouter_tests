# node.js

### leitura e escrita de arquivos com fs
  -  terceiro parâmentro flag opcional
      - `w` para escrita
      - `wx` lança errro caso o arquivo exista
  - leitura
```js
const fs = require('fs').promises;
const path = require('path');

async function readData() {
  try {
    const json = await fs.readFile(path.resolve(__dirname, './meu-arquivo.txt'), 'utf-8');
    const data = JSON.parse(data);
    return data;
    console.log('arquivo lido com sucesso');
  } catch (err) {
    console.error(`Erro ao ler o arquivo: ${err.message}`);
  }
}
```
  - escrita
```js
 const fs = require('fs').promises;
 const path = require('path');
 
 async function updateFile(id, newData) {
  try {
    const oldFile = await readData();
    // operação com arquivo antigo
    // const index = oldFile.<prop>.findIndex(...);
    // oldFile.<prop>[index] = { id, ...newData };
    const newFile = JSON.stringify(oldFile);
    await fs.writeFile(path.resolve(__dirname, './meu-arquivo.txt'), newFile);
    console.log('Arquivo escrito com sucesso!');
  } catch (err) {
    console.error(`Erro ao escrever o arquivo: ${err.message}`);
  }
}
 ```

## express
### ambiente
  - `npm init -y` 
  - `npm i express express-async-errors@3.1 cors@2.8 morgan`
  - `npm i -D nodemon mocha@10.0 chai@4.3 chai-http@4.3 sinon@14.0` 
    - versões especificas somente para trybe
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
  - criar pastas src e tests. Dentro de src a pasta files, middlewares, routes. Dentro de tests as pastas unit e integration
  - adicionar ao package.json
```js
"main": "src/server.js"
"scripts": {
"start": "node src/server.js",
"dev": "nodemon src/server.js",
"lint": "eslint --no-inline-config --no-error-on-unmatched-pattern -c .eslintrc.json .",
"lint:fix":"eslint --fix --ext .js,.jsx ." // adicionar extenções desejadas,
"tests":"mocha tests/**/*.test.js --exit"
},
```

### inicializando
```js
// src/app.js
const express = require('express');
require('express-async-errors');
const app = express();
app.use(morgan('dev'));
app.use(express.json());
// ...
module.exports = app;

// src/server.js
const app = require('./app');
app.listen(3001, () => console.log('server running on port 3001'));

// tests/integration/foo.test.js
const app = require('../../src/app')
const chai = require('chai');
const chaiHttp = require('chai-http');
const sinon = require('sinon');
const fs = require('fs').promisses;

const { expect } = chai;
chai.use(chaiHttp);
```

### definições
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
} catch(e) {
  res.status(500).json({message: e.message});
}})
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
  
  - parâmentro `req`
    - req.params acessa os paramentros da requisição
    - req.query acessa as queries da requisição
    - req.path acessa o caminho da requisição
    - req.body acessa o corpo da requisição
    
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
      
  - requisições com body
    - desestruturar as propriedades do body e depois re-estruturar. (para não trazer mais informações do que o necessário)
```js
app.put('<route>', async (req, res) => {
  try {
    const { id } = req.params;
    const { name, brandId } = req.body;
    // ...
  } catch (e) {
    if (...) return res.status(404).json({ message: ""});
    return res.status(500).end();
  }
})
```

- middlewares
  -  podem ser criados (função) ou instalados (pacote) e aplicados globalmente ou na chamada da rota
  - ex: `express.json(), express.static(<path>), morgan('dev'), cors()`
    - static: permite servir arquivos estáticos em um caminho automaticamente
    - morgan: produx logs da aplicação
    - cors: permite definir politica de cors. Conexão entre front e back 
  - middlewares globais podem ser definidos em `app.use(<middleware>)`. Somente as rotas abaixo dessa declaração passarão por esse middleware
  - middlewares locais são definidos como uma função que tem acesso aos paramentros `req, res, next` e podem fazer uma lógica e responder a requisição ou  passar para o proximo middleware com `next()`
  -  Express já vem com um middleware de erro padrão pronto para lidar com a maior parte dos casos comuns através de um retorno em html. Este middleware só entra em ação se a requisição chegar ao final do código sem ser respondida. Caso queira reconfigurar a mensagem de resposta basta definir no final do código um app.use com uma chamada de rota com a mensagem desejada. Ex. `app.use((req, res) => res.sendStatus(404))`
```js
// global
app.use(express.json());

// local
function meuMiddleware(req, res, next) {
// ...
next();
}
app.get('<rota>', meuMiddleware, (req, res) => {
// ...
});
```
  - middleware de erro
    - sempre devem vir depois de rotas e outros middlewares
    - só recebem requisições se algum middleware lançar um erro ou chamar next(err)
    - sempre devem receber quatro parâmetros
    - O Express utiliza a quantidade de parâmetros que uma função recebe para determinar se ela é um middleware de erro ou um middleware comum
    - pode ser encadeado passando `err` como parametro de `next()`, neste caso o express encaminha a requisição para o próximo middleware de erro ao invez de seguir o fluxo normal da rota
```js
app.use((err, _req, res, _next) => {
  res.status(500).json({ message: `Algo deu errado! Mensagem: ${err.message}` });
  
// ou

app.use((err, _req, _res, next) => {
  console.error(err.stack);
  // passa o erro para o próximo middleware
  next(err);
});

app.use((err, _req, res, _next) => {
  res.status(500).json({ message: `Algo deu errado! Mensagem: ${err.message}` });
});
```
  - middleware de rota
    - `const router = express.Router()`
    - o Router é um middleware que “agrupa” várias rotas em um mesmo lugar
```js
// src/app.js

const express = require('express');
require('express-async-errors');
const morgan = require('morgan');
// require no nosso novo router
const teamsRouter = require('./routes/teamsRouter');

const app = express();
app.use(morgan('dev'));
app.use(express.static('/images'));
app.use(express.json());
// monta o router na rota /teams
app.use('/teams', teamsRouter);

app.use((err, _req, _res, next) => {
  console.error(err.stack);
  next(err);
});

app.use((err, _req, res, _next) => {
  res.status(500).json({ message: `Algo deu errado! Mensagem: ${err.message}` });
});

module.exports = app;
```

### testes com mocha
  - recomenda-se não usar arrow function, e sim declarar atraves da palavra function
  - funções `beforeEach() e afterEach()` para setup e teardown
  - AAA (triple A) - técnica para escrita de testes - arrange / act / assert
```js
describe('Foo', function () {
  beforeEach(function() {
  });
  afterEach(function() {
  });
  it('Foo', async function () {
    const output = <output-esperado>;
    const response = await chai
      .request(app)
      .put(<route>)
      .send(<dados-do-body>);
    expect(response.status).to.be.equals(200);
    expect(response.body).to.have.property(<property>);
    expect(response.body).to.deep.equal(output);
  });
})
```
#### mocks com Sinon

- Stubs são objetos que podemos utilizar para simular interações com dependências externas ao que estamos testando de fato
- o escopo da chamada ao sinon deve ser o mais local o possível (dentro do it) pois se mais de um teste usa aquela função haverá conflito entre os retornos
- `sinon.stub(<modulo>, '<função>').resolves(<valor-retornado>)`
- `sinon.restore()` para teardown do stub (aftereach)
```js
// ex. com função readFile do módulo fs

const fs = require('fs');
const mockFile = JSON.stringify({ 
  brands: [
    {
      id: 1,
      name: 'Lindt & Sprungli',
    }]
});

// describe('teste API', function () {
// describe('Usando o método GET em /', function () {
//  it('Retorna a lista completa', async function () {
      sinon.stub(fs.promises, 'readFile').resolves(mockFile);
      expect(fs.readFile.called).to.be.true;
//  });
// });
```
