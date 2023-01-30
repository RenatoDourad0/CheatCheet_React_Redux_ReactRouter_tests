# EsLint

## docs
- [docs](https://eslint.org/docs/latest/user-guide/getting-started)

## inicializando
- responder as perguntas para gerar o arquivo base
- ```npm init @eslint/config```

## modelo arquivo de configuração
- react
 ```json
```
- express
```json
```

- express com ts
 - `npm i -D @typescript-eslint/eslint-plugin @typescript-eslint/parser eslint eslint-config-airbnb-typescript eslint-plugin-import` 
```json
// .eslintrc.json
{
  "root": true,
  "env": {
    "browser": false,
    "node": true,
    "es2021": true,
    "jest": true
  },
  "extends": [
    "plugin:@typescript-eslint/recommended",
    "airbnb-base",
    "plugin:mocha/recommended",
    "airbnb-typescript/base"
  ],
  "parser": "@typescript-eslint/parser",
  "parserOptions": {
    "ecmaVersion": 2019,
    "sourceType": "module",
    "project": "./tsconfig.json"
  },
  "plugins": [
    "@typescript-eslint"
  ],
  "rules": {
    "no-console": "off",
    "consistent-return": "off",
    "@typescript-eslint/no-unused-vars": [
      "error",
      {
        "argsIgnorePattern": "^_",
        "ignoreRestSiblings": true
      }
    ]
  }
}
```

## scripts e comandos
- para auto correção adicionar este script ao package.json (confirmar se os arquivos não necessarios de lint estão na chave eslinignore do package.json ou no .eslintignore)
- ```"lint:fix":"eslint --fix ./*.js"```
