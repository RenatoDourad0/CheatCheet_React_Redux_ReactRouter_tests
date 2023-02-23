falta -
definicoes react: hooks / context / ciclo de vida / patterns
moks: mock na mão / mockServiceWorker
css: grid, animações, pseudo-classes e pseudo-seletores, responsividade, mobile first, sass, modulos css, jquary, bootstrap, bulma, tailwind, styled-components,  material ui, Figma

# CheatSheet React, ReactRouter, Redux e testes
  
  * Com base no módulo de front-end da Trybe
  * Por [Renato Campos](https://github.com/RenatoDourad0)

## Índice
  * [LocalStorage](#localStorage)
    * [LocalStoage com redux](#LocalStoage-com-redux)
  * [React](#React)
    * [Instalação](#Instalação-React)    
    * [No diretório `src`](#No-diretório-(/src))
    * [Importações comuns](#Importações-comuns-React)
    * [Componentes de classe](#Componentes-de-classe)
      * [Ciclo de vida](#Ciclo-de-vida)
    * [Componentes funcionais](#Componentes-funcionais)
      * [Hooks](#Hooks)
    * [Renderização condicional](#Renderização-condicional)
    * [PropTypes](#proptypes)
  * [ReactRouter](#ReactRouter)
    * [Instalação](#Instalação-ReactRouter)
    * [No arquivo `index.js`](#No-arquivo-(index.js))
    * [No diretório `src/components`](#No-diretório-(src/components))
    * [No arquivo `src/App.js`](#No-arquivo-(src/App.js))
    * [Importações comuns](#Importações-comuns-ReactRouter)
    * [History](#history) 
    * [Rotas dinâmicas](#Rotas-dinâmicas)
    * [Rota notFound](#rota-notfound)
  * [Redux](#Redux)
    * [Instalação](#Instalação-Redux)
    * [No diretório `src`](#No-diretório-src)
    * [No diretório `redux`](#No-diretório-redux)
    * [No diretório `reducers`](#No-diretório-reducers)
    * [No arquivo `store.js`](#No-arquivo-(store.js))
    * [No diretório `actions`](#No-diretório-actions)
        * [Actions assíncronas com thunk](#Actions-assíncronas-com-thunk)
          * [No arquivo `src/redux/index.js`](#No-arquivo-(src/redux/index.js))
          * [No arquivo `src/redux/actions/{sua_action}`](#No-arquivo-(src/redux/actions/{sua_action}))
    * [No arquivo `src/index.js`](#No-arquivo-(src/index.js))
    * [Nos componentes que vão ler o estado](#Nos-componentes-que-vão-ler-o-estado)
    * [Nos componentes que vão modificar o estado](#Nos-componentes-que-vão-modificar-o-estado)
  * [Resumo de instalações e importações](#Resumo-de-instalações-e-importações)
    * [React](#Resumo-React)
    * [ReactRouter](#Resumo-ReactRouter)
    * [Redux](#Resumo-Redux)
  * [Ambiente de testes](#Ambiente-de-testes)
    * [Node Assert](#node-assert)
    * [Jest](#jest)
      * [Moks](#moks)
      * [`fetch-mock`](#fetch-mock)
    * [RTL](#rtl)
      * [RTL com ReactRouter](#rtl-com-reactrouter)
      * [RTL com Redux](#rtl-com-redux)
        * [Testes assíncronos com Redux](#testes-assíncronos-com-redux)
      * [RTL com ReactRouter e Redux](#rtl-com-reactrouter-e-redux)
  * [Estilização](#Estilização)
    * [fontAwesome](#fontAwesome)
    * [Bulma](#bulma)
    * [Sass](#Sass)


## LocalStorage

1. Para salvar dados não primitivos
  * `localStorage.setItem('item', JSON.stringify(data))`

2. Para buscar dados não primitivos
  * `JSON.parse(localStorage.getItem('item'))`

3. Para limpar dados
  * limpar todos os dados - `localStorage.clear()`
  * limpar um item - `localStorage.removeItem('item')`

### LocalStoage com redux

1. Usar a função `subscribe` do `store` para atualizar as informações salvas no `localStorage`
  * No arquivo `redux/store.js` 
    * Chamar a `store.subscribe(() => { })` e dentro dela a `store.getState('item')` para buscar as informações salvas no store.
    * Chamar `JSON.parse(localStorage.getItem('item'))` e comparar se houve mudança entra a informação do store e do localStorage.
    * caso tenha tido mudanças chamar `localStorage.setItem('item', JSON.stringify(storeData))` para atualizar o localStorage

```javascript
// src/redux/store.js
{ ... }
store.subscribe(() => {
  const storeData = store.getState();
  const localStorageData = JSON.parse(localStorage.getItem('item'));
  if (storeData.item.length > localStorageData.length) {
    localStorage.setItem('item', JSON.stringify(storeData))
  }
})
```

## React

### Instalação React

1. ``` npx create-react-app nome-do-projeto ``` 
2. ``` cd nome-do-projeto ``` 
3. ``` npm start ```

### No diretório `/src`

1. Criar diretórios ```components, pages, context, hooks, test e styles``` e organizar os arquivos existentes

### Importações comuns React

1. 
  * ``` import React from 'react' ```
  * ```import ReactDOM from 'react-dom' ```
  * ``` import { Component, ... } from 'react'; ```
  * ``` import PropTypes from 'prop-types' ```

### Componentes de classe

1. 

#### Ciclo de vida

1. `componentDidMount`
2. `shouldComponentUpdate`
3. `componentDidUpdate`
4. `conponentWillUnmount`

### Componentes funcionais

1. 

#### Hooks

1. `useState`
2. `useEffect`
3. `useCallback`
4. Hooks do ReactRouter
  - `useHistory`
  - `useLocation`
5. Hooks do Redux
  -  
  -

### Context

1. Com classes
  -

2. Com funções
  -

### PropTypes

1. 

### Renderização condicional

1.


## ReactRouter

### Instalação ReactRouter

1. Versão 5: ``` npm install --save react-router-dom@v5 ```
2. A versão 6 teve breaking changes 

### No arquivo `index.js`

1. Importar `BrowserRouter` e envolver o `App`.

```javascript
// src/index.js
import React from 'react';
import ReactDOM from 'react-dom/client';
import { BrowserRouter } from 'react-router-dom';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <React.StrictMode>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </React.StrictMode>
);
```

### No diretório `src/components`

1. Criar o arquivo `Routes.js` e importar `Route` e `Switch`
  * Importar componentes/páginas referentes as rotas
  * Rotas que renderizam componentes sem props podem utilizar o `component={ Componet }`. Rotas que renderizam componentes que precisam de props devem usar a `render={ (props) => <Componet { ...props } (+ outras props) /> }`

```javascript
// src/components/Routes.js
import { Route, Switch } from 'react-router-dom';
import Component1 from '../pages/Component1'
import Component2 from '../pages/Component2'
import Component3 from '../pages/Component3'
import NotFound from '../pages/NotFound'

function Routes(props) {
  const { name } = props;
  return (
      <Switch>
        <Route
          exact
          path="/"
          render={ (props) => <Component1 { ...props } name={ name } /> }
        />
        <Route exact path="/js" component={ Component2 } />
        <Route exact path="/react" component={ Component3 } />
        <Route component={ NotFound } />
      </Switch>
  )
};

export default Routes;
```

2. Criar o arquivo `Navegation.js` e importar `Link`

```javascript
// src/components/Navegation.js
import { Link } from 'react-router-dom';

function Navegation() {
  return (
      <nav>
        <Link to="/">Home</Link>
        <Link to="/js">JavaScript</Link>
        <Link to="/react">React</Link>
      </nav>
  )
};

export default Navegation;
```

### No arquivo `src/App.js`

1. No arquivo `src/App.js` importar e renderizar os componentes `Routes` e `Navegation`
2. Local ideial para fazer um componente de navegação

```javascript
// src/App.js
import Routes from "./components/Routes.js";
import Navegation from "./components/Navegation.js"

function App() {
  return (
      <Routes />
  );
}

export default App;
```

### Importações comuns ReactRouter

1. ``` import {BrowserRouter, Route, Switch, Link, Redirect, ...} from 'react-router-dom' ```

### `history`

1. O `history` é um objeto que fica fica disponível através de props a todos os componentes renderizados por rotas
2. Permite o carregamento de novas rotas através de métodos como `push()`, `replace()`, `goForward()`, `goBack()` e `block()`
3. Permite acessar o caminho da url atual através da propriedade `location.pathname`

### Rotas dinâmicas

1. Rotas dinâmicas permitem que o componente que é renderizado pela rota tenha acesso em suas props ao valor passado para a URL 
2. Informação fica dfisponível na prop match -> params -> placeholder (id)
  * ``` const { match: { params: { id } } } = props ```

```javascript
// src/components/Routes.js
  <Route
    exact
    path="/registers/:id"
    render={ (props) => <Component1 { ...props } name={ name } /> }
  />
  // ou
  <Route exact path="/registers/:id" component={ Component1 } />
```

### Rota notFound
  * Basta fazer uma `Route` sem `path` na ultima posição do `Switch`, renderizando o componente NotFound

## Redux

### Instalação Redux

1. ``` npm install redux react-redux ```
2. Devtools extension: ``` npm install --save @redux-devtools/extension ```
3. middlewares comuns: ``` npm install --save redux-logger / npm install redux-thunk ```

### No diretório `src`
1. Criar o diretório `redux`

### No diretório `redux`
1. Criar o arquivo `store.js`
2. Criar o diretório `actions`
3. Criar o diretório `reducers`

### No diretorio `reducers`
1. Criar arquivos individuais para cada `reducer` necessário. Exemplo: `userReducer.js`
2. Em cada um dos `reducer` criar os casos para suas actions (switch/case), retornando o estado atualizado.
3. Importar os `actionTypes` (exemplo adiante) refrentes ao `reducer`

```javascript
// src/redux/reducers/userReducer.js
import { GET_USER_DATA, /actions types a serem usados/ } from '../redux/actions/actionTypes'

const INITIAL_STATE = {
  userData: undefined,
  isLogged: false,
};

function userReducer(state = INITIAL_STATE, action) {
  switch (action.type) {
    case GET_USER_DATA:
      return {
        ...state,
        userData: action.payload,
        isLogged: true,
        };
    default:
      return state;
  }
}

export default nameReducer1;
```

2. Criar o arquivo `index.js` e nele o `rootReducer` usando a função `combineReducers` importada do `redux` e os reducers criados anteriormente

```javascript
// src/redux/reducers/index.js
import { combineReducers } from 'redux';
import userReducer from './userReducer';
import nameReducer2 from './nameReducer2';

const rootReducer = combineReducers({ userReducer. nameReducer2 });

export default rootReducer;
```

### No arquivo `store.js`
1. Importar o `rootReducer`
2. Importar o `DevTools` do `@redux-devtools/extension`
3. Importar `createStore` e `applyMiddleware` do `redux`
4. Importar midleweres necessários (ex: `thunk` e `logger`)
5. Chamar a função `createStore` passando como parâmentro o `rootReducer` e a `composeWithDevtools`
6. (opcional) Como parâmetro da função `composeWithDevtools` chamar a função `applyMiddleware`, passando os middlewares desejados

```javascript
// src/redux/store/index.js
import { createStore, applyMiddleware } from 'redux';
import { composeWithDevTools } from '@redux-devtools/extension';
import logger from 'redux-logger'; // console.log() mudanças de estado do redux

import rootReducer from '../reducers';

const store = createStore(rootReducer, composeWithDevTools(applyMiddleware(logger)));

export default store;
``` 

7. Funções `subscribe` e `getState` do `store`
  * A ``` store.subscribe() ``` permite que uma ação seja executada sempre após o estado ser alterado.
  * ``` store.getState() ``` permite acessar o estado do store

```javascript
// src/redux/store/index.js

{...}
const store = createStore(rootReducer, composeWithDevTools(applyMiddleware(logger)));

store.subscribe(() => {
  const state = store.getState();
  const { wallet: { expenses } } = state;
  const savedExpenses = JSON.parse(localStorage.getItem('expenses')) || [];
  if (expenses.length !== 0) {
    localStorage.setItem('expenses', JSON.stringify(expenses))
  };
  if (expenses.length === 0 && savedExpenses.length === 1) {
    localStorage.setItem('expenses', JSON.stringify(expenses))
  };
})

export default store;
``` 

### No diretorio `actions`
1. Criar arquivo `actionTypes.js` com as actionTypes sendo exportadas (pensar no nome da action de acordo com a ação que a gerou).

```javascript
// src/redux/actions/actionTypes.js
export const GET_USER_DATA = 'GET_USER_DATA';
export const SOMAR_NUMEROS = 'SOMAR_NUMEROS';
```

2. Criar as action Creator (separar as actions creators de diferentes reducers em arquivos diferentes)
3. Importar os actionTypes necessários

```javascript
// src/redux/actions/mathActions.js
import { SOMAR_NUMEROS, DIVIDIR_NUMEROS } from '../actions/actionTypes';

export const somarNumerosAction = (payload) => ({ type: SOMAR_NUMEROS, payload });
export const dividirNumerosAction = (payload) => ({ type: DIVIDIR_NUMEROS, payload });

// src/redux/actions/userActions.js
import { GET_USER_DATA } from '../actions/actionTypes';

export const getUserDataAction = (payload) => ({ type: GET_USER_DATA, payload });
```

#### Actions assíncronas com thunk

##### No arquivo `src/redux/index.js`

1. Instalar o middleware redux-thunk: ```npm install redux-thunk```
2. Verificar a existencia da função `applyMiddleware` do `redux` e caso necessário fazer a importação
2. Importar o `thunk` do `redux-thunk`
3. acrescentar a `applyMiddleware` como parâmetro da função `composeWithDevtools`, passando em seus argumentos o middleware `thunk`

```javascript
// src/redux/store/index.js
import { createStore, applyMiddleware } from 'redux';
import { composeWithDevTools } from '@redux-devtools/extension';
import thunk from 'redux-thunk'; // Actions assíncronas
import logger from 'redux-logger'; // console.log() mudanças de estado do redux

import rootReducer from '../reducers';

const store = createStore(rootReducer, composeWithDevTools(applyMiddleware(thunk, logger)));

export default store;
``` 

##### No arquivo `src/redux/actions/{sua_action}`

1. O `thunk` permite que as action creators façam mais do que retornar uma simples `action`. As ultimas funções deste exemplo mostram seu funcionamento
  * As action creators assíncronas retornam uma função anônima que tem acesso ao método `dispatch`, a partir dai é possível chamar novas action creators síncronas que alteram o estado do redux com os dados do processo assíncrono
  * Repare que somente a função `fetchJsSubredditAction` é exportada, usando das outras action creators após cada passo do processo assincrono ( primeiro `dispatch` - altera estado `isLoading` / segundo `dispatch` - sava o retorno da api no estado em caso de sucesso / terceiro `dispatch` - salva o erro da api no estado em caso de falha )
  * (boa pratica ?) É possível também retornar o dado assíncrono diretamente, sem passar para um actionCreator síncrono salvar no `store`. Para isso é preciso importar e usar a função no componente , como no caso da `fetchExchangeRatesAction`

```javascript
import { REQUEST_JS_SUBREDDIT, GET_JS_SUBREDDIT_DATA, GET_JS_SUBREDDIT_ERROR} from'./action_types';

const requestJsSubredditAction = () => ({
  type: REQUEST_JS_SUBREDDIT,
});

const getJsSubredditDataAction = (payload) => ({
  type: GET_JS_SUBREDDIT_DATA,
  payload
});

const getJsSubredditErrorAction = (payload) => ({
  type: GET_JS_SUBREDDIT_ERROR,
  payload
});

export const fetchJsSubredditAction = (subreddit) => {
  return (dispatch) => {
    dispatch(requestJsSubreddit());
    return fetch(`https://www.reddit.com/r/${subreddit}.json`)
      .then((response) => response.json())
      .then((data) => dispatch(getJsSubredditData(data)))
      .catch((error) => dispatch(getJsSubredditError(error)))
  }
};

export const fetchExchangeRatesAction = () => () => fetch('https://economia.awesomeapi.com.br/json/all')
  .then((response) => response.json())
  .then((data) => (data));
```

### No arquivo `src/index.js`
1. Importar o `Provider` e envolver o `<App />`
2. Importar o `store` e passar como prop do `<Provider />`

```javascript
// src/index.js
import React from 'react';
import ReactDOM from 'react-dom';
import { BrowserRouter } from 'react-router-dom'; // se necessário
import { Provider } from 'react-redux';
import App from './App';
import store from './store';

ReactDOM.render(
  <React.StrictMode>
    <BrowserRouter> // se necessário
      <Provider store = { store }>
        <App />
      </Provider>
    <BrowserRouter>
  </React.StrictMode>,
  document.getElementById('root'),
);
``` 

### Nos componentes que vão ler o estado:
1. criar a função `mapStateToProps`
2. importar `connect` do `react-redux`
3. exportar o componente usando `connect()()` 

```javascript
// src/components/FirstComponent
import { connect } from 'react-redux'
...
const mapStateToProps = state => ({
  myFirstState: state.myReducer.state,
  });

export default connect(mapStateToProps, null)(FirstComponent);
``` 

4. O componente agora tem acesso ao estado do redux através das props definidas na função `mapStateToProps`
5. Omitindo o segundo argumento da função `connect` o `dispatch` ficará disponível para o componente através de suas props ``` connect(mapStateToProps)(FirstComponent) ```

### Nos componentes que vão modificar o estado:
1. criar a função `mapDispatchToProps`
2. importar `connect`
3. Importar as actions creators necessárias
4. exportar o componete usando `connect()()`

```javascript
// src/components/SecondComponent
import { connect } from 'react-redux'
import { getUserDataAction } from '../redux/actions/actionTypes.js'
...
const mapDispatchToProps = dispatch => ({
  myFirstDispatch: (payload) => dispatch(getUserDataAction(payload)),
  });
  
  export default connect(null, mapDispatchToProps)(SecondComponent);
``` 

5. O componente agora pode alterar o estado do redux através das props definidas na função `mapDispatchToProps`


## Resumo de instalações e importações

### Resumo React
1. Instalação: ``` npx create-react-app nome-do-projeto ``` 
2. Importações

```javascript
import React from 'react' 
import ReactDOM from 'react-dom' 
import { Component, ... } from 'react'
import PropTypes from 'prop-types'
```

3. Diretrizes:
  * Start: ``` npm start ```
  * Test: ``` npm test ``` ou ``` npm test -- --coverage ``` ou ``` npm run test-coverage ``` ou ``` npm run test-coverage -- --collectCoverageFrom=caminho/da/Pagina ``` ou ainda, em caso de uso do Cypress `npm run cy:open`
  * Build: ``` npm run build ```
  * Deploy com gh-pages: 
    * Dentro do diretório do projeto: ``` npm install gh-pages ```
    * No arquivo ``` package.json ``` alterar:

```javascript
// package.json
"homepage" : "https://{ username_github }.github.io/{ nome_do_diretório }",
{ ... }
"scripts": {
          "predeploy" : "npm run build",
          "deploy" : "gh-pages -d build"
        }
```

  * No arquivo ``` Routes.js ``` alterar o path da rota principal de `/` para `/{nome-do-diretório}`
  * 
    * ```npm run deploy```
    * Em caso de erro ``` gh ... já existente ``` apagar a pasta `gh-pages` em ``` node-modules/.cache ```
4. Organização:
  * Diretórios ```components, pages, test e styles```

### Resumo ReactRouter
1. Instalação versão 5: ``` npm install --save react-router-dom@v5 ```
2. Importação: ``` import {BrowserRouter, Route, Switch, Link, Redirect, ...} from 'react-router-dom' ```
3. Diretrizes: 
  *  No arquivo `src/index.js` importar `BrowserRouter` e envolver o componente`App`.
  *  No diretório `src/components` criar o arquivo `Routes.js` e importar `Route` e `Switch`
  * No arquivo `src/App.js` importar e renderizar o componente `Routes`

### Resumo Redux
1. Instalação

 ```bash
npm install redux react-redux
npm install --save @redux-devtools/extension
npm install redux-thunk
npm install --save redux-logger
 ```

2. Importações

 ```javascript
 import { connect, Provider } from 'react-redux'
 import { createStore, combineReducers, applyMidleWare } from 'redux'
 import { composeWithDevTools } from '@redux-devtools/extension'
 import thunk from 'redux-thunk'
 import logger from 'redux-logger'
 ```

3. Diretrizes: 
  * Verificar o passo a passo [Iniciando o Redux](#Iniciando-o-Redux)
4. Organização:
  * Diretório `Redux`
  * Diretórios `store`, `actions`, `reducers`, cada um com seu `index.js`

## Ambiente de testes:

#### Node Assert

1. Importação: ``` const assert = require('assert') ```
2. Assinatura: ``` assert.{ matcher }( { value } ) // true ou false ```
3. Matchers comuns: `strictEqual`, ...

#### Jest

1. Instalação: ``` npm install --save-dev jest ```
  * sujestão de texto: ``` npm install @types/jest ```
2. No `package.json`, na chave `scripts` alterar a chave `test` para `jest`

```javascript
// package.json
  "scripts": {
    "test": "jest",
  },
```

3. Matchers comuns: `toBe`, ...
4. Assinaturas

```javascript
// ./test/index.test.js
describe('bloco de testes', () => {

  beforeEach(() => {
    // antes de cada teste
  });

  afterEach(() => {
    // depois de cada teste
  });

  test('test 1', () => {
    expect(funcXyz).toHaveBeenCalled()
    expect(funcXyz()).toBe(xyz)
  })

// assíncrono

  it('descrição', async () => {
    await requisicaoMokada();
    expect(requisicaoMokada()).toBe(xyz)
  });

})
```

##### Moks

1. Mocks permitem simular o comportamento de funções reais e com retorno imprevisivel de forma previsivel.

###### `fetch-mock`

  * Biblioteca auxiliar para mockar o `fetch`.

1. Instalações 

```bash
  npm install node-fetch@^2
  npm install --save-dev fetch-mock-jest
```

2. Importação 

```javascript
  import fetchMock from 'fetch-mock'
```

3. Uso
  * [Documentação](https://www.wheresrhys.co.uk/fetch-mock/#api-mockingmock)
  * [CheatSheet](https://github.com/wheresrhys/fetch-mock/blob/master/docs/cheatsheet.md)
  * Normalmente basta importar
  * Chamada
    * `fetchMock.getOnce(URL, { retorno experado })` é a forma mais simples de mockar o `fetch` de um get uma vez durante aquele bloco de teste. 
      * `URL` é o endpoit chamado na implementação original e `retorno esperado` a resposta esperada da requisição original
    * existem outras formas de se usar, consultar cheatSheet.
  * Asserção
    * `fetchMock.called()` retorna true ou false se fetch tiver sido invocada
    * `fetchMock.called(URL)` retorna true ou false se fetch tiver sido invocada com a URL especificada
    * `fetchMock.lastCall()`, `fetchMock.lastUrl()` ou `fetchMock.lastOptions()` permite acessar os parametros passados a última chamada do fetch
  * Teardown
    * `fetchMock.resetHistory()` reset do histórico de chamadas
    * `fetchMock.reset()` ou `fetchMock.restore()` restaura o `fetch()` a sua implementação original
    * fazer o `fetchMock.restore()` no `afterEach()` dos blocos `describe`
    * Importante! O `fetchMock.getOnce()` mocka somente uma chamada ao `fetch`. Então se o teste faz mais de uma chamada deve ser feito um `fetchMock.restore()` entre estas chamadas e uma nova implementação deve ser feita (um novo `fetchMock.getOnce()`) 

```javascript
  fetchMock.getOnce('https://dog.ceo/api/breeds/image/random', {
      body: { message: 'myDogUrl' },
    });
  
  userEvent.click(buttonDoguinho);
  await waitFor(() => expect(fetchMock.called()).toBeTruthy());

  fetchMock.restore();

  fetchMock.getOnce('https://dog.ceo/api/breeds/image/random', {
      body: { message: 'myDogUrl' },
    });

  userEvent.click(buttonDoguinho);
  await waitFor(() => expect(fetchMock.called()).toBeTruthy());
  fetchMock.restore();
```

#### RTL

* [CheatSheet](./cheat-sheet-RTL.pdf)

1. Com o ```npx create-react-app ``` a RTL, a userEvent e o Jest serão instaladas
2. Caso seja necessário instalação

```bash
npm install --save-dev @testing-library/react
npm install --save-dev @testing-library/user-event
npm install --save-dev jest 
```

3. O arquivo `setupTests.js` também é criado pelo `create-react-app` e fornece para os testes os chamados custom jest matchers. O .toBeInTheDocument() é um exemplo.
4. Importações

```javascript
// ./tests/index.test.js
import React from 'react';
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import Component from '{ path to Component }';
```

5. Assinaturas

```javascript
// ./test/index.test.js
test('renders the Component', () => {
  render(<Component />)

  const heroText = screen.getByRole('heading', { level: 1, name: /{ texto hero }/i})
  const heroBtn = screen.getByRole('button', { name: /{ texto button }/i})

  expect(heroText).toBeInTheDocument()
  expect(heroText).toHaveTextContent('hero text')

  userEvent.click(heroBtn)

  expect(heroText).toHaveTextContent('new hero text')
})
```

6. Renderizando componentes específicos
  * É possível renderizar qualquer componente da aplicação mas caso o componente espere receber props, é preciso passar estas props no `render`. Por isso é mais facil renderizar sempre o `App` e navegar a partir dele para o componente desejado através da `userEvent`
7. Seletores e matchers comuns: 
  * Verificar a [CheatSheet](./cheat-sheet-RTL.pdf)
  * Sempre dar preferência aos seletores de `role`, expecificando seu tipo e seu texto através da propriedade name
  * Referência para `role`: [MDN Aria Roles](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles)
8. Métodos comuns da userEvent: ``` userEvent. click(element) / type(element, text) / ... ```
  * O método `userEvent.type()` aceita como terceiro argumento um objeto que tem a chave delay, um tempo em ms entre cada digitação. Caso seja preenchido este método se torna assíncrono.

```javascript
// ./test/index.test.js
test('renders the Component', async () => {
  render(<Component />)

  const heroInput = screen.getByRole('textbox', { name: /{ texto input }/i })

  expect(heroInput).toBeInTheDocument()

  await waitFor(() => userEvent.type(heroInput, 'Renato', { delay: 50 })) // precisa do waitFor?

  expect(heroText).toHaveTextContent('new hero text')
})
```

##### RTL com ReactRouter

1. Tudo dito sobre RTL continua válido
2. A forma de renderizar o componente muda para se ter acesso as propriedades do ReactRouter
3. Agora é possível navegar entre rotas através da `userEvent` (cliques) e do `history` (`history.push()`) e verificar a url atual através do `history.location.pathname`
4. Função auxiliar `renderWithRouter`
  * Importante ressaltar que não é necessário utilizar a função para realizar os testes. Em alguns casos de testes simples essa renderização especial pode ser feita de forma direta no próprio teste.
    * Para isso basta criar um histórico mockado e chamar a função `render` envolvedo o componente a ser renderizado em um `Route`
    * É possivel passar a rota a ser renderizada na propriedade `initialEntries`, um array de strings
```javascript
// src/tests/App
import React from 'react';
import { Router } from 'react-router-dom';
import { createMemoryHistory } from 'history';
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import Component from '{ path to component }'

test('description', () => {
const initialEntries = ['/'];
const history = createMemoryHistory({ initialEntries });
render(
  <Router history={ history }>
    {component}
  </Router>
  );
  {...}
})
```

  * `renderWithRouter`

```javascript
// src/tests/helpers/renderWithRouter.js
import React from 'react';
import { Router } from 'react-router-dom';
import { createMemoryHistory } from 'history';
import { render } from '@testing-library/react';

const renderWithRouter = (component, initialEntries = ['/']) => {
  const history = createMemoryHistory({ initialEntries });
  return ({
    ...render(<Router history={ history }>{component}</Router>), history,
  });
};
export default renderWithRouter;
```
  * A função `render` agora está 'turbinada' com um mock do `history` da aplicação

5. Importações

```javascript
// tests/App.test.js
import React from 'react';
import { screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import renderWithRouter from './helpers/renderWithRouter';
import Component from '{ path to Component }';
```

6. Assinatura da função `renderWithRouter`

```javascript
// tests/App.test.js
it('descrição', () => {
  const { history } = renderWithRouter(<App />);
  const { pathname } = history.location;
});

it('descrição', () => {
  const { history } = renderWithRouter(<App />, ['/login']);
  const { pathname } = history.location;
});
```

  * O primeiro argumento é o componente a ser renderizado
  * O segundo argumento é a rota a ser renderizada, um array de strings
  * Se descontroi a propriedade `history` na chamada da função

7. Assinatura dos testes

```javascript
// tests/App.test.js
it('deve renderizar o componente Sobre', () => {
  const { history } = renderWithRouter(<App />);

  const aboutLink = screen.getByRole('link', { name: 'Sobre' });
  expect(aboutLink).toBeInTheDocument();
  userEvent.click(aboutLink);

  const { pathname } = history.location;
  expect(pathname).toBe('/about');

  const aboutTitle = screen.getByRole('heading',
    { name: 'Você está na página Sobre' });
  expect(aboutTitle).toBeInTheDocument();
});

it('deve testar um caminho não existente e a renderização do Not Found', () => {
    const { history } = renderWithRouter(<App />);

    history.push('/pagina/que-nao-existe/');

    const notFoundTitle = screen.getByRole('heading',
      { name: 'Página não encontrada' });
    expect(notFoundTitle).toBeInTheDocument();
  });

  it('descrição', () => {
  const { history } = renderWithRouter(<App />, ['/login']);
  const { pathname } = history.location;
  expect(pathname).toBe('/login');
});
```

##### RTL com Redux

1. Tudo dito sobre RTL continua válido
2. A forma de renderizar o componente muda para se ter acesso ao estado do Redux
3. Agora é possível acessar a variável `store` através da descontrução da função `renderWithRedux()`
4. O `Provider` deve estar no `src/index.js` e não no `src/App.js`, se não é impossivel renderizar o App com o mock do `store` disponível 
5. Função auxiliar `renderWithRedux`
  * Importante se atentar para a estrutura do `store` original da aplicação, se tiver sido implementado com a função `combineReducer` (rootReducer) o mock também deve seguir essa implementação
  * Vale resaltar que assim como a `renderWithRouter()` não é necessário utilizar a `renderWithRedux()` sempre. Em caso de testes simples o mock pode ser feito diretamente no teste
  * Sem o uso da função `combineReducers`

```javascript
// src/tests/helpers/renderWithRedux.js
import React from 'react';
import { createStore } from 'redux';
import { Provider } from 'react-redux';
import { render } from '@testing-library/react';
import rootReducer from './src/redux/reducers/index'

const renderWithRedux = (
  component,
  { 
    initialState = {}, 
    store = createStore(rootReducer, initialState),
  } = {}
) => ({
    ...render(
      <Provider store={store}>
        {component}
      </Provider>
      ),
    store,
});

export default renderWithRedux
```
  * Ou diretamente no arquivo de testes

```javascript
// src/tests/App.test.js
import React from 'react';
import { createStore } from 'redux';
import { Provider } from 'react-redux';
import rootReducer from './src/redux/reducers/index'
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import Component from '{ path to component }'

test('description', () => {
  const initialState = {};
  const store = createStore(rootReducer, initialState);
  render(
    <Provider store={ store }>
      <Component />
    </Provider>
    );
    {...}
});
```

  * Com o uso da função `combineReducers`

```javascript
// src/tests/helpers/renderWithRedux.js
import React from 'react';
// import { createStore } from 'redux';
import { createStore, combineReducers } from 'redux';
import { Provider } from 'react-redux';
import { render } from '@testing-library/react';
// import rootReducer from './src/redux/reducers/index'
import reducer1 from './src/redux/reducers/reducer1'
import reducer2 from './src/redux/reducers/reducer2'

const renderWithRedux = (
  component,
  { 
    initialState = {}, 
    // store = createStore(rootReducer, initialState),
    store = createStore(combineReducers({ reducer1, reducer2 }), initialState),
  } = {}
) => ({
    ...render(
      <Provider store={store}>
        {component}
      </Provider>
      ),
    store,
})

export default renderWithRedux
```

  * Ou diretamente no arquivo de testes

```javascript
// src/tests/App.test.js
import React from 'react';
import { createStore, combineReducers } from 'redux';
import { Provider } from 'react-redux';
import Reducer1 from './src/redux/reducers/Reducer1'
import Reducer2 from './src/redux/reducers/Reducer2'
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import Component from '{ path to component }'

test('description', () => {
  const initialState = {};
  const store = createStore(combineReducers({ reducer1, reducer2 }), initialState);
  render(
    <Provider store={ store }>
      <Component />
    </Provider>
    );
    {...}
});
```
* A função `render` agora está 'turbinada' com um mock do `store` da aplicação

6. Importações

```javascript
// tests/App.test.js
import React from 'react';
import { cleanup, screen } from '@testing-library/react';
import renderWithRedux from './helpers/renderWithRedux'
import userEvent from '@testing-library/user-event';
import Componet from '{ path to Component }';
```

  * O `cleanUp` limpa da memória o que tiver sido renderizado na função render. É um comportamento automático no Jest, mas pode ser necessário.

7. Assinatura da função `renderWithRedux`

```javascript
// tests/App.test.js
test('descrição', () => {
  renderWithRedux(<Component />)
  // ou com estado inicial
  renderWithRedux(<Componet />, { initialState: { reducer1: { state1: value } } })
})
```

  * Quando chamada sem o segundo argumento trás para o `store` os valores definidos no `INITIAL_STATE` da aplicação
  * O segundo argumento deve ser um objeto que respeita a estrutura ``` { initialState: { nomeReducer }: { nomePropriedade: valorPropriedade }} ``` no caso do store ter sido definido com a função `combineReducers` ou ``` { initialState: { nomePropriedade: valorPropriedade }} ``` no caso do store ter somente um reducer
8. Assinatura dos testes

```javascript
// tests/App.test.js
describe('testing clicks', () => {
  beforeEach(cleanup);
  test('the page should have a button and a text 0', () => {
    renderWithRedux(<App />);
    const buttonAdicionar = screen.queryByText('Clique aqui');

    expect(buttonAdicionar).toBeInTheDocument();
    expect(screen.getByText('0')).toBeInTheDocument();
  });

  test('a click in a button should increment the value of clicks', () => {
    const { store } = renderWithRedux(<App />, { initialState: { clickReducer: { counter: 5 } } });

    expect(store.clickReducer.counter).toBe(5);
    expect(screen.getByText('5')).toBeInTheDocument();
  });
});
```

###### Testes assíncronos com Redux

  1. A função `renderWithRedux()` deve ser tratada com o `thunk` para aceitar comportamento assíncrono 

```javascript
// src/tests/helpers/renderWithRedux.js
import React from 'react';
import { createStore, combineReducers, applyMiddleware } from 'redux';
import { Provider } from 'react-redux';
import thunk from 'redux-thunk';
import { render } from '@testing-library/react';
import reducer1 from './src/redux/reducers/reducer1'
import reducer2 from './src/redux/reducers/reducer2'

const renderWithRedux = (
  component,
  { 
    initialState = {}, 
    store = createStore(combineReducers({ reducer1, reducer2 }), initialState, applyMiddleware(thunk)),
  } = {}
) => ({
    ...render(
      <Provider store={store}>
        {component}
      </Provider>
      ),
    store,
})

export default renderWithRedux
```

2. Estrutura do teste

```javascript
// src/App.test.js
import { waitFor, screen, cleanup } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import React from 'react';
import fetchMock from 'fetch-mock-jest';
import App from './App';
import renderWithRedux from './helper';

describe('Página principal', () => {
  beforeEach(cleanup);
  test('Testa que o botão de adicionar cachorro está presente', async () => {
    renderWithRedux(<App />);
    const buttonDoguinho = screen.queryByText('Novo Doguinho');

    expect(buttonDoguinho).toBeInTheDocument();

    fetchMock.getOnce('https://dog.ceo/api/breeds/image/random', {
      body: { message: 'myDogUrl' },
    });

    userEvent.click(buttonDoguinho);
    await waitFor(() => expect(fetchMock.called()).toBeTruthy());

    fetchMock.reset()
  });
});
```

##### RTL com ReactRouter e Redux

1. Tudo dito sobre RTL continua válido
2. A forma de renderizar o componente muda para se ter acesso as propriedades do ReactRouter como `history` e ao estado do Redux
3. Agora é possível navegar entre rotas através da `userEvent` (cliques) e do `history` (`history.push()`), verificar a url atual através do `history.location.pathname` e ter acesso ao `store`
4. Função auxiliar `renderWithRouterAndRedux`

```javascript
// src/tests/helpers/renderWithRouterAndRedux.js
import React from 'react';
import { Router } from 'react-router-dom';
import { createMemoryHistory } from 'history';
import { render } from '@testing-library/react';
import { Provider } from 'react-redux';
import { createStore, (applyMiddleware) } from 'redux';
(import thunk from "react-thunk")
import rootReducer from '../../redux/reducers';

const renderWithRouterAndRedux = (
  component,
  {
    initialState = {},

    // sem thunk
    store = createStore(rootReducer, initialState),
    // com thunk 
    store = createStore(rootReducer, initialState, applyMiddleware(thunk)),

    initialEntries = ['/'],
    history = createMemoryHistory({ initialEntries }),
  } = {},
) => ({
  ...render(
    <Router history={ history }>
      <Provider store={ store }>
        {component}
      </Provider>
    </Router>,
  ),
  history,
  store,
});

export default renderWithRouterAndRedux;
```
  * A função `render` agora está 'turbinada' com mocks do `history` e do `store` da aplicação
  5. Importações

```javascript
// tests/App.test.js
import React from 'react';
import { cleanup, screen, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import renderWithRedux from './helpers/renderWithRedux'
import fetchMock from 'fetch-mock-jest';
import Componet from '{ path to Component }';
```

6. Assinatura da função `renderWithRouterAndRedux`

```javascript
// tests/App.test.js
import store from "../redux/store.js"

const I_S = {...}

// cria um novo store com o estado inicial da aplicação e um novo history com initialEntries '/'
const { history, store } = renderWithRouterAndRedux(<App />);

// cria um novo store com o estado inicial passado como argumento e um novo history com initialEntries '/'
const { history, store } = renderWithRouterAndRedux(<App />, { initialState: I_S });

// cria um novo store com o estado inicial passado como argumento e um novo history com initialEntries igual a passada como argumento
const { history, store } = renderWithRouterAndRedux(<App />, {
  initialState: I_S,
  initialEntries: ['/login'],
  });

// Utiliza o store original da aplicação e cria um novo history com initialEntries igual a passada como argumento
const { history, store } = renderWithRouterAndRedux(<App />, {
store: store,
initialEntries: ['/login'],
  });
```

7. Estrutura dos testes

```javascript
// tests/App.test.js
import React from 'react';
import { cleanup, screen, waitFor } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import renderWithRouterAndRedux from './helpers/renderWithRedux'
import fetchMock from 'fetch-mock-jest';
import Componet from '{ path to Component }';

describe('Página principal', () => {
  beforeEach(cleanup);

  const initialState = {};
  const mockData = {};

  test('a despesa é adicionada ao store se preenchido corretamente', async () => {
    const { history, store } = renderWithRouterAndRedux(<App />, {
      initialState: initialState,
      initialEntries: ['/carteira'],
    });

    fetchMock.getOnce(URL, { ...mockData });

    const valueInputField = screen.getByLabelText(/valor:/i);
    const descriptionInputField = screen.getByLabelText(/Descrição:/i);
    const submitButton = screen.getByRole('button', { name: /adicionar despesa/i });

    userEvent.type(valueInputField, '10');
    userEvent.type(descriptionInputField, 'a');

    userEvent.click(submitButton);

    await waitFor(() => expect(fetchMock.calls().length).toBe(1));

    fetchMock.restore();

    const expese1 = screen.getAllByText('a');
    const expese2 = screen.getAllByText('10.00');

    expect(expese2.length).toBe(1);
    expect(expese1.length).toBe(1);
    expect(store.wallet.expeses.length).toBe(1);
  });
});
```

## Estilização

### fontAwesome

1. [Documentação](https://fontawesome.com/start)

#### Com CDN

#### Com React

1. Instalação

```javascript
npm install --save @fortawesome/fontawesome-svg-core // pacote base
npm install --save @fortawesome/free-solid-svg-icons // icones sólidos
npm install --save @fortawesome/free-regular-svg-icons // icones comuns
npm install --save @fortawesome/react-fontawesome@latest // componente react
```

2. Importação
  * pesquisar na documentação o nome do icone a ser importado

```javascript
import { faEnvelope, faKeyboard, nome_do_icone } from '@fortawesome/free-regular-svg-icons';
import { faWallet, faCoins } from '@fortawesome/free-solid-svg-icons';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome'
```

3. Uso

```javascript
<FontAwesomeIcon icon={faCoins} />
```

  * verificar opções de estilização [aqui](https://fontawesome.com/docs/web/use-with/react/style)

### Bulma

1. [Documentação](https://bulma.io/documentation/overview/start/)


### Sass

1. [Documentação](https://sass-lang.com/guide)

2. Organização
  * criar pasta `styles`
    * criar arquivo `main.scss` - aonde serão importados todos os arquivos
    * criar arquivo `_base.scss` - estilos globais, css reset
    * criar arquivo `_colors.scss` - definição de cores
    * criar arquivo `components.scss` - estilo de componentes específicos
    * criar arquivo `layout.scss` - posicionamento e forma dos componentes

3. Instalação

```javascript
npm install sass
```

4. Importação
  * A ordem de importação é importante.

```css
/* /src/styles/main.scss */

@import '_colors.scss';
@import 'base.scss';
@import 'components.scss';
@import 'layout.scss';
```

```css
/* /src/index.js */

import './styles/main.scss';
```

4. Uso
  * Variaveis

```css
/* /src/styles/_colors.scss */
$nome_da_variavel: #ffffff;

/* /src/styles/components.scss */
.component_class {
  background: $nome_da_variavel;
}
```

  * Nesting

```css
/* /src/styles/components.scss */

a {
  display: block;
  padding: 6px 12px;

  &:hover {
    color: red;
  }

  strong {
    font-weight: bolder;
  }
  }
```

  * Outras ferramentas
    * Mixins - funções para calcular propriedades com base em um parâmetro
    * Operators - fazer contas

