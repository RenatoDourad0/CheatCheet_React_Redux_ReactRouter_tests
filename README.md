falta actions assincronas, testes, resumo de importacoes
# Criando um projeto com React, Redux e ReactRouter

## Índice
  * [Instalando e iniciando o React](#Instalando-e-iniciando-o-React)
  * [Instalando o Redux](#Instalando-o-Redux)
  * [Instalando e iniciando o ReactRouter](#Instalando-e-iniciando-o-ReactRouter)
  * [Iniciando o Redux](#Iniciando-o-Redux)
    * [Criar no diretório `redux`](#Criar-no-diretório-redux)
    * [Dentro do diretório `reducers`](#Dentro-do-diretrio-reducers)
    * [Dentro do diretório `store`](#Dentro-do-diretório-store)
    * [Dentro do diretório `actions`](#Dentro-do-diretório-actions)
        * [Caso especial - Actions assíncronas](#Caso-especial---Actions-assíncronas)
          * No arquivo `src/redux/index.js`
          * No arquivo `src/redux/actions/{sua_action}`
    * [No arquivo `src/index.js`](#No-arquivo-src-index-.-js)
    * [Nos componentes que vão ler o estado](#Nos-componentes-que-vão-ler-o-estado)
    * [Nos componentes que vão modificar o estado](#Nos-componentes-que-vão-modificar-o-estado)
  * [Resumo de instalações, importações e organização diretórios](#Resumo-de-instalações,-importações-e-organização-diretórios)
    * [React](#React)
    * [ReactRouter](#ReactRouter)
    * [Redux](#Redux)
  * [Ambiente de testes](#Ambiente-de-testes)


## Instalando e iniciando o React

1. ``` npx create-react-app nome-do-projeto ``` 
2. ``` cd nome-do-projeto ``` 
3. ``` npm start ```
4. Criar diretórios ```components, pages, test e styles``` e organizar os arquivos existentes

## Instalando o Redux

1. ``` npm install redux react-redux ```
2. Devtools extension: ``` npm install --save @redux-devtools/extension ```
3. middlewares comuns: ``` npm install --save redux-logger / npm install redux-thunk ```
4. criar no diretório `src` o diretório `redux`

## Instalando e iniciando o ReactRouter

1. ``` npm install react-router-dom@v5 ```
2. no arquivo `index.js` importar `BrowserRouter` e envolver o `App`.

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

3. No diretório `src/components` criar o arquivo `Routes.js` e importar `Route` e `Switch`
4. Importar componentes/páginas referentes as rotas
```javascript
// src/components/Routes.js
import { Route, Switch } from 'react-router-dom';
import Component1 from '../pages/Component1'
import Component2 from '../pages/Component2'
import Component3 from '../pages/Component3'

function Routes() {
  return (
      <Switch>
        <Route exact path="/" component={ Component1 } />
        <Route exact path="/js" component={ Component2 } />
        <Route exact path="/react"component={ Component3 } />
      </Switch>
  )
};

export default Routes;
```

6. No arquivo `src/App.js` importar e renderizar o componente `Routes`

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

## Iniciando o Redux

### Criar no diretório `redux`:
1. criar o diretório `store`
2. criar o diretório `actions`
3. criar o diretório `reducers`

### Dentro do diretório `reducers`
1. Criar arquivos individuais para cada `reducer` necessário. Exemplo: `userReducer.js`
2. Em cada um dos `reducer` criar os casos para suas actions (switch/case), retornando o estado atualizado.
3. Importar os `actionTypes` (explicação adiante) refrentes ao `reducer`

```javascript
// src/redux/reducers/userReducer.js
import { GET_USER_DATA, /actions types a serem usados/ } from '../redux/actions/actionTypes'

const INITIAL_STATE = {
  userData: undefined,
};

function userReducer(state = INITIAL_STATE, action) {
  switch (action.type) {
    case GET_USER_DATA:
      return {
        ...state,
        userData: action.payload,
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

### Dentro do diretório `store`
1. Criar o arquivo `index.js`
2. Importar o `rootReducer`
3. Importar o `DevTools` do `@redux-devtools/extension`
4. Importar `createStore` e `applyMiddleware` do `redux`
5. Importar midleweres necessários (ex: `thunk` e `logger`)
6. Chamar a função `createStore` passando como parâmentro o `rootReducer` e a `composeWithDevtools`
7. Como parâmetro da função `composeWithDevtools` chamar a função `applyMiddleware`, passando os middlewares desejados (opcional)

```javascript
// src/redux/store/index.js
import { createStore, applyMiddleware } from 'redux';
import { composeWithDevTools } from '@redux-devtools/extension';
import logger from 'redux-logger'; // console.log() mudanças de estado do redux

import rootReducer from '../reducers';

const store = createStore(rootReducer, composeWithDevTools(applyMiddleware(logger)));

export default store;
``` 

### Dentro do diretório `actions`
1. Criar arquivo `actionTypes.js` com as actionTypes sendo exportadas (pensar no nome da action de acordo com a ação que a gerou).

```javascript
// src/redux/actions/actionTypes.js
export const GET_USER_DATA = 'GET_USER_DATA';
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

#### Caso especial - Actions assíncronas:
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
1. 

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
3. Importar as actions necessárias
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

## Resumo de instalações, importações e organização diretórios:

### React
1. Istalação:
2. Importação: 
3. Organização:

### ReactRouter
1. Istalação:
2. Importação: ``` import {BrowserRouter, Route, Switch, Link, Redirect, ...} from 'react-router-dom' ```
3. Organização:

### Redux
1. Istalação:
2. Importação:
3. Organização:

## Ambiente de testes:




