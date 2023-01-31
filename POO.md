# POO

## Diagramas UML
- diagrama de casos de uso

## SOLID
- Sigle responsability
  - 'um módulo deve ser responsável por somente um ator'
  - por módulo se entende classe / método
  - por ator se entende aquele que age sobre / utiliza o módulo
- open closed
  - 'Você deve ser capaz de estender um comportamento de uma entidade sem modificar seus comportamentos já existentes' 
- liskov substitution
- interface segregation
- dependecy invertion
  - Injeção de dependêcia: passar dependências como parâmetro ao invéz de hard coded na classe, dando mais flexibilidade ao código.
  - Inversão de dependencia: passar dependencia como parametro e tipar este parametro com uma abstração genérica o suficiente para que possa receber outras implementações daquela dependecia
  - 'Módulos de alto nível não devem depender de módulos de baixo nível (não se deve fazer hard code de dependecias em uma classe). Ambos devem depender de abstrações(dependem de interfaces genéricas que permitem implementar um grupo de dependecias especificas). Abstrações não devem depender de detalhes (interfaces genéricas não devem depender de detalhes de suas sub-classes). Detalhes devem depender de abstrações(um grupo de sub-classes devem depender da mesma interface genérica)' 

## Express e sequelize com POO
- tsconfig
```json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "es2019",
    "rootDir": "./src",
    "outDir": "./build",
    "esModuleInterop": true,
    "strict": true
  }
}
```
- .eslintrc
```json
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
- dependecias (cors/ async-errors / joy / jwt / helmet / ...)
  - `npm i -D @types/express @types/node @typescript-eslint/eslint-plugin @typescript-eslint/parser eslint eslint-config-airbnb-typescript eslint-plugin-import ts-node typescript` 
  - `npm i express express-async-errors joy json-web-token cors helmet`
- app
- server
```ts
/app.ts
import express, { Request, Response } from 'express';
import PlantRouter from './router/PlantRouter';
import errorMiddleware from './middlewares/errorMiddleware';
import 'express-async-errors';
import 'cors';
import 'helment';

export default class App {
  public app: express.Express;

  constructor() {
    this.app = express();

    this.config();
    this.routes();

    this.app.get('/', (req: Request, res: Response) => res.json({ ok: true }));
    this.app.use(errorMiddleware);
  }

  private routes(): void {
    this.app.use('/plants', PlantRouter);
  }

  private config():void {
    const accessControl: express.RequestHandler = (_req, res, next) => {
      res.header('Access-Control-Allow-Origin', '*');
      res.header('Access-Control-Allow-Methods', 'GET,POST,DELETE,OPTIONS,PUT,PATCH');
      res.header('Access-Control-Allow-Headers', '*');
      next();
    };
    
    this.app.use(cors());
    this.app.use(helmet());
    this.app.use(express.json());
    this.app.use(accessControl);
  }

  public start(PORT: string | number):void {
    this.app.listen(PORT, () => console.log(`Running on port ${PORT}`));
  }
}
/server.ts
import App from './App';

const PORT = 3001;

new App().start(PORT);
```
- /routes
- /middlewares
```ts
// /errorMiddleware.ts
import { ErrorRequestHandler } from 'express';
import HttpException from '../exceptions/HttpException';

const errorMiddleware: ErrorRequestHandler = (err, _req, res, _next) => {
  console.log('err', err);
  const { status, message } = err as HttpException;
  res.status(status || 500).json({ message });
};

export default errorMiddleware;
```
- /interfaces
```ts
// /Iservice.ts
export interface IServiceReader<T> {
  getAll(): Promise<T[]>;
  getById(id: string): Promise<T | null>
}

export interface IServiceWriter<T, U> {
  create(arg: U): Promise<T>
  update(id: string, arg: Omit<T, 'id'>): Promise<T>
}
export interface IServiceDelete {
  removeById(id: string): Promise<void>
}

export interface IService<T, U> extends
  IServiceReader<T>,
  IServiceWriter<T, U>,
  IServiceDelete {}
  
// /index.ts
export {
  IService,
  IServiceReader,
  IServiceWriter,
  IServiceDelete,
} from './IService';
```
- /errors
```ts
// /httpException.ts

class HttpException extends Error {
  status: number;

  constructor(status: number, message: string) {
    super(message);
    this.status = status;
  }
}

export default HttpException;

// /badRequestException.ts
import HttpException from './HttpException';

export default class BadRequestException extends HttpException {
  private static status = 400;

  constructor(message?: string) {
    super(BadRequestException.status, message || 'Bad request');
  }
}
```
- /validations
```ts
import { INewPlant } from '../../interfaces';
import { BadRequestException } from '../../exceptions';

export default class PlantValidate {
  private static validateBreed(breed: string): void {
    if (typeof breed !== 'string') {
      throw new BadRequestException('Attribute "breed" must be string.');
    }
  }

  private static validateNeedsSun(needsSun: boolean): void {
    if (typeof needsSun !== 'boolean') {
      throw new BadRequestException('Attribute "needsSun" must be boolean.');
    }
  }

  private static validateOrigin(origin: string): void {
    if (typeof origin !== 'string') {
      throw new BadRequestException('Attribute "origin" must be string.');
    }
  }

  private static validateSize(size: number): void {
    if (typeof size !== 'number') {
      throw new BadRequestException('Attribute "size" must be number.');
    }
  }

  public static validateAttributes(plant: INewPlant): void {
    PlantValidate.validateBreed(plant.breed);
    PlantValidate.validateNeedsSun(plant.needsSun);
    PlantValidate.validateOrigin(plant.origin);
    PlantValidate.validateSize(plant.size);
  }
}
```
- ../tests
- /database
- /services
- /controllers
- /auth
