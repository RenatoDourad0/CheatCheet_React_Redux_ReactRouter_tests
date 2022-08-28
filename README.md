falta moks
# Iniciando um projeto com React,ReactRouter e Redux + testes

## Índice
  * [React](#React)
    * [Instalação](#Instalação-React)
    * [No diretório `src`](#No-diretório-(/src))
    * [Importações comuns](#Importações-comuns-React)
    * [Componentes de classe](#Componentes-de-classe)
    * [Componentes funcionais](#Componentes-funcionais)
    * [Ciclo de vida](#Ciclo-de-vida)
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
    * [No diretório `store`](#No-diretório-store)
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
    * [RTL](#rtl)
      * [RTL com ReactRouter](#rtl-com-reactrouter)
      * [RTL com Redux](#rtl-com-redux)
      * [RTL com ReactRouter e Redux](#rtl-com-reactrouter-e-redux)
    * []()


## React

### Instalação React

1. ``` npx create-react-app nome-do-projeto ``` 
2. ``` cd nome-do-projeto ``` 
3. ``` npm start ```

### No diretório `/src`

1. Criar diretórios ```components, pages, test e styles``` e organizar os arquivos existentes

### Importações comuns React

1. 
  * ``` import React from 'react' ```
  * ```import ReactDOM from 'react-dom' ```
  * ``` import { Component, ... } from 'react'; ```
  * ``` import PropTypes from 'prop-types' ```

### Componentes de classe

1. 

### Componentes funcionais

1. 

### Ciclo de vida

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
2. Importar componentes/páginas referentes as rotas
3. Rotas que renderizam componentes sem props podem utilizar o `component={ Componet }`. Rotas que renderizam componentes que precisam de props devem usar a `render={ (props) => <Componet { ...props } (+ outras props) /> }`

```javascript
// src/components/Routes.js
import { Route, Switch } from 'react-router-dom';
import Component1 from '../pages/Component1'
import Component2 from '../pages/Component2'
import Component3 from '../pages/Component3'

function Routes() {
  const name = ...
  return (
      <Switch>
        <Route
          exact
          path="/"
          render={ (props) => <Component1 { ...props } name={ name } /> }
        />
        <Route exact path="/js" component={ Component2 } />
        <Route exact path="/react"component={ Component3 } />
      </Switch>
  )
};

export default Routes;
```
### No arquivo `src/App.js`

1. No arquivo `src/App.js` importar e renderizar o componente `Routes`

```javascript
// src/App.js
import Routes from "./components/Routes";

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

1. O `history` é um objeto que fica fica disponível através de props a todos os componentes renderizados por rotas (?)
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
1. Criar no diretório `redux`

### No diretório `redux`
1. Criar o diretório `store`
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

### No diretorio `store`
1. Criar o arquivo `index.js`
2. Importar o `rootReducer`
3. Importar o `DevTools` do `@redux-devtools/extension`
4. Importar `createStore` e `applyMiddleware` do `redux`
5. Importar midleweres necessários (ex: `thunk` e `logger`)
6. Chamar a função `createStore` passando como parâmentro o `rootReducer` e a `composeWithDevtools`
7. (opcional) Como parâmetro da função `composeWithDevtools` chamar a função `applyMiddleware`, passando os middlewares desejados

```javascript
// src/redux/store/index.js
import { createStore, applyMiddleware } from 'redux';
import { composeWithDevTools } from '@redux-devtools/extension';
import logger from 'redux-logger'; // console.log() mudanças de estado do redux

import rootReducer from '../reducers';

const store = createStore(rootReducer, composeWithDevTools(applyMiddleware(logger)));

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

1. O `thunk` permite que as action creators façam mais do que retornar uma simples `action`. A ultima função deste exemplo mostra seu funcionamento.
2. As action creators assíncronas retornam uma função anônima que tem acesso ao método `dispatch`, a partir dai é possível chamar novas action creators comuns
3. Repare que somente a função `fetchJsSubreddit` é exportada, usando das outras action creators após cada passo do processo assincrono ( primeiro `dispatch` - altera estado `isLoading` / segundo `dispatch` - sava o retorno da api no estado em caso de sucesso / terceiro `dispatch` - salva o erro da api no estado em caso de falha )

```javascript
import { REQUEST_JS_SUBREDDIT, GET_JS_SUBREDDIT_DATA, GET_JS_SUBREDDIT_ERROR} from'./action_types';

const requestJsSubreddit = () => ({
  type: REQUEST_JS_SUBREDDIT,
});

const getJsSubredditData = (payload) => ({
  type: GET_JS_SUBREDDIT_DATA,
  payload
});

const getJsSubredditError = (payload) => ({
  type: GET_JS_SUBREDDIT_ERROR,
  payload
});

export const fetchJsSubreddit = (subreddit) => {
  return (dispatch) => {
    dispatch(requestJsSubreddit());
    return fetch(`https://www.reddit.com/r/${subreddit}.json`)
      .then((response) => response.json())
      .then((data) => dispatch(getJsSubredditData(data)))
      .catch((error) => dispatch(getJsSubredditError(error)))
  }
};
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
  myFirstDispatch: (state) => dispatch(getUserDataAction(state)),
  });
  
  export default connect(null, mapDispatchToProps)(SecondComponent);
``` 

5. O componente agora pode alterar o estado do redux através das props definidas na função `mapDispatchToProps`


## Resumo de instalações e importações

### Resumo React
1. Istalação: ``` npx create-react-app nome-do-projeto ``` 
2. Importações

```javascript
import React from 'react' 
import ReactDOM from 'react-dom' 
import { Component, ... } from 'react'
import PropTypes from 'prop-types'
```

3. Diretrizes:
  * Start: ``` npm start ```
  * Test: ``` npm test ``` ou ``` npm test -- --coverage ``` ou ``` npm run test:coverage ```
  * Build: ``` npm run build ```
  * Deploy com gh-pages: 
    * Dentro do diretório do projeto: ``` npm install gh-pages ```
    * No arquivo ``` package.json ``` alterar:
      * ``` "homepage" : "https://{username_github}.github.io/{nome_do_diretório}" ```
      * Na propriedade `scripts`:

```javascript
// package.json
"scripts": {
          "predeploy" : "npm run build",
          "deploy" : "gh-pages -d build"
        }
```
  * 
    * ```npm run deploy```
    * Em caso de erro ``` gh ... já existente ``` apagar a pasta `gh-pages` em ``` node-modules/.cache ```
4. Organização:
  * Diretórios ```components, pages, test e styles```

### Resumo ReactRouter
1. Istalação versão 5: ``` npm install --save react-router-dom@v5 ```
2. Importação: ``` import {BrowserRouter, Route, Switch, Link, Redirect, ...} from 'react-router-dom' ```
3. Diretrizes: 
  *  No arquivo `src/index.js` importar `BrowserRouter` e envolver o componente`App`.
  *  No diretório `src/components` criar o arquivo `Routes.js` e importar `Route` e `Switch`
  * No arquivo `src/App.js` importar e renderizar o componente `Routes`

### Resumo Redux
1. Istalação: ``` npm install redux react-redux ```
2. Importações

 ```javascript
 import { connect, Provider } from 'react-redux'
 import { createStore, combineReducers, applyMidleWare } from 'redux'
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

#### RTL

* [CheatSheet](./cheat-sheet-RTL.pdf)

1. Com o ```npx create-react-app ``` a RTL e a userEvent serão instaladas
2. Caso seja necessário istalação
```bash
npm install --save-dev @testing-library/react
npm install --save-dev @testing-library/user-event
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
7. Seletores e matchers comuns: Verificar a [CheatSheet](./cheat-sheet-RTL.pdf)
  * Sempre dar preferência aos seletores de `role`, expecificando seu tipo e seu texto através da propriedade name
  * Referência para `role`: (MDN Aria Roles)[https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles]
8. métodos da userEvent: ``` userEvent. click(element) / type(element, text) / ... ```
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

1. Todo dito sobre RTL continua válido
2. A forma de renderizar o componente muda para se ter acesso as propriedades do ReactRouter como `history` e `location.pathname`
3. Agora é possível navegar entre rotas através da `userEvent` (cliques) e do `history` (`history.push()`) e verificar a url atual através do `history.location.pathname`
3. Função auxiliar `renderWithRouter`

```javascript
// src/tests/helpers/renderWithRouter.js
import React from 'react';
import { Router } from 'react-router-dom';
import { createMemoryHistory } from 'history';
import { render } from '@testing-library/react';

const renderWithRouter = (component) => {
  const history = createMemoryHistory();
  return ({
    ...render(<Router history={ history }>{component}</Router>), history,
  });
};
export default renderWithRouter;
```
  * a função `render` agora está 'turbinada' com um `history` local

4. Assinatura dos testes

```javascript
// tests/App.test.js
import React from 'react';
import { screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import renderWithRouter from './helpers/renderWithRouter';
import App from './App';

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
```

##### RTL com Redux

##### RTL com ReactRouter e Redux







