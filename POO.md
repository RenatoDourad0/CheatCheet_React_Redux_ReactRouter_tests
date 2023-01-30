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
- dependecias (cors/ async-errors / joy / jwt / helmet / ...)
- app
- server
```ts
import express, { Request, Response } from 'express';
import PlantRouter from './router/PlantRouter';
import errorMiddleware from './middlewares/errorMiddleware';

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

    this.app.use(express.json());
    this.app.use(accessControl);
  }

  public start(PORT: string | number):void {
    this.app.listen(PORT, () => console.log(`Running on port ${PORT}`));
  }
}
```
- /routes
- /middlewares
- /auth
- /interfaces
- /validations
- ../tests
- /database
- /services
- /controllers
