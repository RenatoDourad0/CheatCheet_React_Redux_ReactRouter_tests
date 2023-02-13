# POO

## Diagramas UML
- diagrama de casos de uso

## SOLID
- Sigle responsability
  - 'um módulo deve ser responsável por somente um ator'
  - por módulo se entende classe / método
  - por ator se entende aquele que age sobre / utiliza o módulo
  - resumo:
    - O Single Responsibility Principle (SRP) é um princípio importante da programação orientada a objetos que diz que cada classe deve ter apenas uma responsabilidade única. Isso significa que cada classe deve ser projetada para realizar uma tarefa específica e não múltiplas tarefas.
    - O objetivo do SRP é garantir que as classes sejam pequenas, simples e fáceis de entender, modificar e testar. Isso é alcançado dividindo as classes em pequenos módulos que realizam tarefas específicas. Desta forma, quando uma classe precisa ser modificada, só precisamos nos preocupar com as tarefas relacionadas a essa classe, em vez de ter que lidar com todas as tarefas relacionadas a várias classes diferentes. 
    - Um exemplo de violação do SRP seria uma classe "Conta Bancaria" que tem responsabilidade de fazer operações de saque, depósito e consulta do extrato e também faz operações de transferencia. Nesse caso, a classe "Conta Bancaria" estaria violando o princípio SRP, pois ela estaria lidando com mais de uma responsabilidade. A solução seria criar classes diferentes para cada operação como "ContaBancariaSaque", "ContaBancariaDeposito", "ContaBancariaExtrato" e "ContaBancariaTransferencia".   
```ts
class ContaBancaria {
    private saldo: number;
    constructor(saldo: number) {
        this.saldo = saldo;
}
    public sacar(valor: number) {
        if (valor > this.saldo) {
            throw new Error("Saldo insuficiente");
        }
        this.saldo -= valor;
    }
    public depositar(valor: number) {
        this.saldo += valor;
} }
// E aqui está o exemplo da classe "ContaBancariaExtrato
class ContaBancariaExtrato {
    private contaBancaria: ContaBancaria;
    constructor(contaBancaria: ContaBancaria) {
        this.contaBancaria = contaBancaria;
}
    public consultarExtrato(): number {
        return this.contaBancaria.saldo;
} }
```
- open closed
  - 'Você deve ser capaz de estender um comportamento de uma entidade sem modificar seus comportamentos já existentes'
  - resumo:
    - OCP (Open-Closed Principle) é um dos princípios SOLID que diz que "as classes devem ser abertas para extensão, mas fechadas para modificação". Isso significa que você deve ser capaz de adicionar novos comportamentos às classes sem modificar o código existente.
    - Para seguir OCP, você deve:
      - Isolar as partes do código que podem mudar e permitir que elas sejam extendidas sem afetar o resto do código.
      - Criar interfaces ou classes abstratas para definir comportamentos comuns e permitir que as classes concretas implementem esses comportamentos de forma diferente.
    - Para usar o metodo OCP é sempre necessario que uma interface seja criada?
      - Não necessariamente. O uso de interfaces é uma maneira comum de seguir o princípio OCP, mas existem outras maneiras de implementar esse princípio também.
      - Uma das maneiras de seguir OCP é criar classes abstratas que definem comportamentos comuns e permitem que as classes concretas implementem esses comportamentos de forma diferente. Outra maneira é usar herança para permitir que classes filhas estendam a funcionalidade de classes pais sem modificar o código existente.
      - A ideia principal do OCP é permitir que novos comportamentos sejam adicionados às classes sem modificar o código existente. Então, é importante que a estrutura do código seja projetada de maneira flexível e escalável para permitir a extensão sem causar modificações no código existente
```ts
// interface
interface Forma {
    area(): number;
}
class Quadrado implements Forma {
    private lado: number;
constructor(lado: number) {
    this.lado = lado;
}
public area(): number {
    return this.lado * this.lado;
    }
}
class Circulo implements Forma {
    private raio: number;
    private PI = 3.14;constructor(raio: number) {
    this.raio = raio;
}
public area(): number {
    return this.PI * this.raio * this.raio;
    }
}

let quadrado = new Quadrado(5);
console.log(quadrado.area()); // imprime 25
let circulo = new Circulo(2);
console.log(circulo.area()); // imprime 12.56
let formas: Forma[] = [quadrado, circulo];
formas.forEach(forma => {
    console.log(forma.area());
});

// classe abstrata
abstract class Veiculo {
    protected velocidade: number;
    constructor(velocidade: number) {
        this.velocidade = velocidade;
}
    public acelerar(): void {
        this.velocidade += 10;
}
    public abstract frear(): void;
}
class Carro extends Veiculo {
    public frear(): void {
        this.velocidade -= 10;
    }

}
class Moto extends Veiculo {
    public frear(): void {
        this.velocidade -= 5;
    }
}

let carro = new Carro(0);
carro.acelerar();
console.log(carro.velocidade); // prints 10
carro.frear();
console.log(carro.velocidade); // prints 0
let moto = new Moto(0);
moto.acelerar();
console.log(moto.velocidade); // prints 10
moto.frear();
console.log(moto.velocidade); // prints 5
```
- dependecy invertion
  - Injeção de dependêcia: passar dependências como parâmetro ao invéz de hard coded na classe, dando mais flexibilidade ao código.
  - Inversão de dependencia: passar dependencia como parametro e tipar este parametro com uma abstração genérica o suficiente para que possa receber outras implementações daquela dependecia
  - 'Módulos de alto nível não devem depender de módulos de baixo nível (não se deve fazer hard code de dependecias em uma classe). Ambos devem depender de abstrações(dependem de interfaces genéricas que permitem implementar um grupo de dependecias especificas). Abstrações não devem depender de detalhes (interfaces genéricas não devem depender de detalhes de suas sub-classes). Detalhes devem depender de abstrações(um grupo de sub-classes devem depender da mesma interface genérica)' 
  - resumo:
    - Dependency Inversion Principle (DIP) é um dos princípios SOLID que diz que "dependa de abstrações, não de implementações". Isso significa que as classes devem depender de interfaces ou classes abstratas em vez de depender de classes concretas.
    - A idéia principal por trás do DIP é que as classes devem ser projetadas de tal forma que elas não estejam fortemente acopladas a outras classes específicas. Ao invés disso, elas devem depender de interfaces ou classes abstratas que definem comportamentos comuns. Isso permite que as dependências sejam trocadas facilmente sem afetar o código existente. 
    - No exemplo abaixo a classe "Servico" depende da interface "IRepositorio" e não de uma implementação específica. Isso significa que podemos passar qualquer implementação de IRepositorio para o construtor da classe Servico sem afetar o seu comportamento. Ao fazer isso, podemos trocar facilmente.
    - Ao usar a classe Servico como intermediária, você está criando uma camada adicional de abstração que pode ser útil em vários cenários. A classe Servico é uma camada de serviço que pode conter lógica de negócios, validação ou qualquer outra lógica adicional além de apenas obter os dados. Dessa forma, se você precisar adicionar mais lógica no futuro, você só precisaria modificar a classe Servico e não as classes de repositório. Além disso, a classe Servico também pode ser reutilizada em várias partes do seu aplicativo, enquanto as classes de repositório só podem ser usadas em um único lugar.
    - Outra vantagem é que, se você precisar trocar a forma como os dados são obtidos (por exemplo, mudar de um banco de dados para um repositório web), você só precisaria modificar a instanciação da classe Servico, sem precisar modificar o resto do código. Isso facilita a manutenção e escalabilidade do código.
```ts
interface IRepositorio {
    obterDados(): string;
}
class RepositorioWeb implements IRepositorio {
    obterDados(): string {
return "Dados obtidos do repositório web";
    }
class RepositorioBancoDeDados implements IRepositorio {
    obterDados(): string {
        return "Dados obtidos do banco de dados";
    }
}
class Servico {
    private repositorio: IRepositorio;
    constructor(repositorio: IRepositorio) {
        this.repositorio = repositorio;
}
    public obterDados(): string {
        //adicionando validação antes de obter os dados
        if(this.validarUsuario()){
            return this.repositorio.obterDados();
        }
        return "Usuário não autorizado";
    }
    //método de validação
    private validarUsuario(): boolean {
        // lógica de validação
        return true;
} }
let servico = new Servico(new RepositorioBancoDeDados());
console.log(servico.obterDados());
```
- liskov substitution
  - Objetos em um programa devem ser substituíveis por instâncias de seus subtipos, sem alterar a funcionalidade do programa.   
- interface segregation
  - Nenhum cliente deve ser forçado a depender de métodos que não utiliza   

## Express e sequelize com POO
- referencia projeto express / sequelize / mysql / js - https://github.com/RenatoDourad0/CheatSheets/blob/main/node(express_mysql2_sequelize_jwt_tests)).md
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
- sequelize
	- após gerar arquivo de configuração rodar o comando `npx tsc && npx sequelize-cli init:models` para gerar o index.js dos models
```ts
// src/database/config/config.js (sequelize)
import 'dotenv/config';
import { Options } from 'Sequelize';

const config: Options = {
  username: process.env.MYSQL_USER,
  password: process.env.MYSQL_PASSWORD,
  database: process.env.MYSQL_DATABASE,
  host: process.env.MYSQL_HOST,
  port: Number(process.env.MYSQL_PORT),
  dialect: 'mysql',
};

module.exports = {
  development: config,
  test: config,
  production: config,
};
	
// /.sequelizerc
import 'path';

module.exports = {
  'config': path.resolve(__dirname, 'build', 'database', 'config', 'config.js'),
  'models-path': path.resolve(__dirname, 'build', 'database', 'models'),
  'seeders-path': path.resolve(__dirname, 'build', 'database', 'seeders'),
  'migrations-path': path.resolve(__dirname, 'build', 'database', 'migrations'),
};
```
- dependecias (cors/ async-errors / morgan / bodyParser / dotenv / joy / jwt / bcrypt / helmet / sequelize)
  - `npm i -D @types/express @types/node @types/sequelize @typescript-eslint/eslint-plugin @typescript-eslint/parser eslint eslint-config-airbnb-typescript eslint-plugin-import ts-node nodemon typescript sequelize-cli sequelize-test-helpers mocha@10.0 chai@4.3 chai-http@4.3 sinon@14.0 sinon-chai nyc@15.1` 
  - `npm i express express-async-errors express-rate-limit joy jsonwebtoken bcrypt dotenv cors helmet morgan body-parser sequelize`
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
import 'morgan';
import 'body-parser';
import rateLimit from 'express-rate-limit';

const limiter = rateLimit({
	windowMs: 15 * 60 * 1000, // 15 minutes
	max: 100, // Limit each IP to 100 requests per `window` (here, per 15 minutes)
	standardHeaders: true, // Return rate limit info in the `RateLimit-*` headers
	legacyHeaders: false, // Disable the `X-RateLimit-*` headers
})

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
    
    this.app.use(bodyParser.json());
    this.app.use(bodyParser.urlencoded({ extended: true }));
    this.app.use(morgan('dev'));
    this.app.use(cors());
    this.app.use(helmet());
//  this.app.use(express.json());
    this.app.use(accessControl);
    this.app.use(limiter);
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
```ts
import { Router } from 'express';
import PlantController from '../controllers/PlantController';
import PlantModel from '../models/PlantModel';
import PlantService from '../services/PlantService';

const plantModel = new PlantModel();
const plantService = new PlantService(plantModel);
const plantController = new PlantController(plantService);

const plantRouter = Router();

plantRouter.get('/', (req, res, next) => plantController.getAll(req, res, next));
plantRouter.post('/', (req, res, next) => plantController.create(req, res, next));
plantRouter.get('/:id', (req, res, next) => plantController.getById(req, res, next));
plantRouter.delete('/:id', (req, res, next) => plantController.remove(req, res, next));
plantRouter.put('/:id', (req, res, next) => plantController.update(req, res, next));

export default plantRouter;
```
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
- /exceptions
```ts
// Httpexception.ts
export default class HttpException extends Error {
  constructor(public status: number, public message: string) {
    super(message);
  }
}

// NotFoundError.ts
import HttpException from './HttpException';

export default class NotFoundError extends HttpException {
  constructor(message:string) {
    super(404, message);
  }
}
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

// Iplant
export interface IPlant {
  id: number,
  breed: string,
  needsSun: boolean,
  origin: string,
  size: number,
  waterFrequency: number,
}

export type INewPlant = Omit<IPlant, 'id' | 'waterFrequency'>;
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
- /services
```ts
import { INewPlant, IPlant } from '../interfaces';
import { IService } from './interfaces';
import { IModel } from '../models/interfaces';
import { NotFoundException } from '../exceptions';
import PlantValidate from './validations/PlantValidate';

class PlantService implements IService<IPlant, INewPlant> {
  private readonly model: IModel<IPlant>;

  constructor(model: IModel<IPlant> = new PlantModel()) {
    this.model = model;
  }

  public async getAll(): Promise<IPlant[]> {
    const plants = await this.model.getAll();
    return plants;
  }

  public async create(plant: INewPlant): Promise<IPlant> {
    PlantValidate.validateAttributes(plant);

    const { needsSun, size, origin } = plant;
    const waterFrequency = needsSun
      ? size * 0.77 + (origin === 'Brazil' ? 8 : 7)
      : (size / 2) * 1.33 + (origin === 'Brazil' ? 8 : 7);

    const newPlant = await this.model.create({ ...plant, waterFrequency });
    return newPlant;
  }
  
  public async getById(id: string): Promise<IPlant> {
    const plant = await this.model.getById(id);
    if (!plant) throw new NotFoundException('Plant not Found!');
    return plant;
  }

  public async update(id: string, plant: Omit<IPlant, 'id'>): Promise<IPlant> {
    const plantExists = await this.model.getById(id);
    if (!plantExists) throw new NotFoundException('Plant not Found!');

    PlantValidate.validateAttributes(plant);

    const editedPlant = await this.model.update({ id: parseInt(id, 10), ...plant });
    return editedPlant;
  }

  public async removeById(id: string): Promise<void> {
    const isPlantRemoved = await this.model.removeById(id);
    if (!isPlantRemoved) throw new NotFoundException('Plant not Found!');
  }
}

export default PlantService;
```
- /controllers
```ts
 import { Request, Response, NextFunction } from 'express';
 import PlantService from '../services/PlantService';

 class PlantController {
  private readonly service: Iservice<Iplant>;

  constructor(service: Iservice<Iplant> = new PlantService()) {
    this.service = service;
  }

 public async getAll(_req: Request, res: Response, next: NextFunction): Promise<Response | void> {
   try {
     const plants = await this.service.getAll();
     return res.status(200).json(plants);
   } catch (error) {
     next(error);
   }
 }

 public async create(req: Request, res: Response, next: NextFunction): Promise<Response | void> {
   try {
     const plant = await this.service.create(req.body);
     return res.status(201).json(plant);
   } catch (error) {
     next(error);
   }
 }

  public async getById(req: Request, res: Response, next: NextFunction): Promise<Response | void> {
    const { id } = req.params;
    try {
      const plant = await this.service.getById(id);
      return res.status(200).json(plant);
    } catch (error) {
      next(error);
    }
  }

  public async remove(req: Request, res: Response, next: NextFunction): Promise<Response | void> {
    const { id } = req.params;
    try {
      await this.service.removeById(id);
      return res.status(204).end();
    } catch (error) {
      next(error);
    }
  }

  public async update(req: Request, res: Response, next: NextFunction): Promise<Response | void> {
    const { id } = req.params;
    try {
      const plant = await this.service.update(id, req.body);
      return res.status(200).json(plant);
    } catch (error) {
      next(error);
    }
  }
 }

 export default PlantController;
```
- models
```ts
// model com sequelize

import { Model, INTEGER, BOOLEAN } from 'sequelize';
import db from '.';
import Team from './Team.model';

class Match extends Model {
  declare id: number;
  declare homeTeamId: number;
  declare homeTeamGoals: number;
  declare awayTeamId: number;
  declare awayTeamsGoals: number;
  declare inProgress: boolean;
}

Match.init({
  id: {
    allowNull: false,
    autoIncrement: true,
    primaryKey: true,
    type: INTEGER,
  },
  homeTeamId: {
    field: 'home_team_id',
    allowNull: false,
    type: INTEGER,
  },
  homeTeamGoals: {
    field: 'home_team_goals',
    allowNull: false,
    type: INTEGER,
    references: {
      model: 'Team',
      key: 'id',
    },
  },
  awayTeamId: {
    field: 'away_team_id',
    allowNull: false,
    type: INTEGER,
  },
  awayTeamsGoals: {
    field: 'away_team_goals',
    allowNull: false,
    type: INTEGER,
    references: {
      model: 'Team',
      key: 'id',
    },
  },
  inProgress: {
    field: 'in_progress',
    allowNull: false,
    type: BOOLEAN,
  },
}, {
  underscored: true,
  sequelize: db,
  modelName: 'Match',
  timestamps: false,
});

Match.belongsTo(Team, { foreignKey: 'homeTeamId', as: 'id' });
Match.belongsTo(Team, { foreignKey: 'awayTeamId', as: 'id' });

Team.hasMany(Match, { foreignKey: 'id', as: 'homeTeamId' });
Team.hasMany(Match, { foreignKey: 'id', as: 'awayTeamId' });

export default Match;


// model com mysql2

import { RowDataPacket, ResultSetHeader, OkPacket } from 'mysql2';
import connection from './connection';
import { IModel } from './interfaces';
import { IPlant } from '../interfaces';

class Mysql2PlantModel implements IModel<IPlant> {
  private conn = connection;

  public async getAll(): Promise<IPlant[]> {
    const query = `SELECT
      id, breed, size, needs_sun as needsSun, origin, water_Frequency as waterFrequency
      FROM plants`;
    const [rows] = await this.conn.execute<RowDataPacket[]>(query);
    const plants = rows as IPlant[];
    return plants;
  }

  public async create(plant: Omit<IPlant, 'id'>): Promise<IPlant> {
    const {
      breed, needsSun, origin, size, waterFrequency,
    } = plant;
    const query = `INSERT INTO plants (breed, needs_sun, origin, size, water_frequency)
      VALUES (?, ?, ?, ?, ?)`;
    const values = [breed, needsSun, origin, size, waterFrequency];

    const [result] = await this.conn.execute<ResultSetHeader>(query, values);

    const newPlant = {
      id: result.insertId,
      ...plant,
    };
    return newPlant;
  }

  public async getById(id: string): Promise<IPlant | null> {
    const query = `SELECT
      id, breed, size, needs_sun as needsSun, origin, water_Frequency as waterFrequency
      FROM plants WHERE id = ?`;
    const values = [id];
    const [rows] = await this.conn.execute(query, values);

    const plantById = rows as IPlant[];
    if (plantById.length === 0) return null;

    return plantById[0];
  }

  public async removeById(id: string): Promise<boolean> {
    const query = 'DELETE FROM plants WHERE id = ?';
    const values = [id];

    const [{ affectedRows }] = await this.conn.execute<OkPacket>(query, values);

    return (affectedRows !== 0);
  }

  public async update(plant: IPlant): Promise<IPlant> {
    const {
      id, breed, needsSun, origin, size, waterFrequency,
    } = plant;

    const query = `UPDATE plants 
      SET breed = ?, needs_sun = ?, origin = ?, size = ?, water_frequency = ?
      WHERE id = ?`;
    const values = [breed, needsSun, origin, size, waterFrequency, id];
    await this.conn.execute<OkPacket>(query, values);

    return plant;
  }
}

export default Mysql2PlantModel;
```
- /auth
```ts
// auth/jwtAuth.ts
import 'dotenv/config';
import { sign, verify, SignOptions, JwtPayload } from 'jsonwebtoken';
import * as bcrypt from 'bcryptjs';
import { BadRequestError, UnauthorizedError, HttpException } from '../errors';
import { IAuthentication } from '../interfaces';
import User from '../database/models/User.model';

type jwtReturn = (JwtPayload & { id: number });

const UnouthorizedErrorMessage = 'Incorrect email or password';

export default class JwtAuth implements IAuthentication {
  private isBodyValid = (email: string, password: string) => email && password;

  private secret = process.env.JWT_SECRET || 'seusecretdetoken';

  async authenticate(email: string, password: string): Promise<(string | HttpException)> {
    if (!this.isBodyValid(email, password)) {
      throw new BadRequestError('All fields must be filled');
    }
    const user = await User.findOne({ where: { email } });
    if (!user || !bcrypt.compareSync(password, user.dataValues.password)) {
      throw new UnauthorizedError(UnouthorizedErrorMessage);
    }
    const { id, username, role, email: dbMail } = user.dataValues;
    const jwtConfig:SignOptions = { expiresIn: '7d', algorithm: 'HS256' };
    const token = sign({ id, username, role, dbMail }, this.secret, jwtConfig);
    return token;
  }

  async validate(token: string): Promise<(User | HttpException)> {
    if (token.length === 0) {
      throw new BadRequestError('All fields must be filled');
    }
    let decoded;
    try {
      decoded = verify(token, this.secret) as jwtReturn;
    } catch (error) {
      throw new UnauthorizedError(UnouthorizedErrorMessage);
    }
    const user = await User.findByPk(decoded.id) as User;
    if (!user || user.dataValues.email !== decoded.dbMail) {
      throw new UnauthorizedError(UnouthorizedErrorMessage);
    }
    return user.dataValues;
  }
}

// middlewares/authentication.ts
import { NextFunction, Request, Response } from 'express';
import { IAuthentication } from '../interfaces';
import JwtAuth from '../auth/JwtAuth';

export default class Authentication {
  constructor(private auth: IAuthentication = new JwtAuth()) {}

  public async authenticate(req: Request, res:Response, next: NextFunction) {
    try {
      const { email, password } = req.body;
      const token = await this.auth.authenticate(email, password);
      return res.status(200).json({ token });
    } catch (error) {
      next(error);
    }
  }

  public async validate(req: Request, res:Response, next: NextFunction) {
    try {
      const token = req.header('Authorization') || '';
      const user = await this.auth.validate(token);
      req.body.user = user;
      return next();
    } catch (error) {
      next(error);
    }
  }
}
```
- ../tests/integration
```ts
import * as sinon from 'sinon';
import * as chai from 'chai';
@ts-ignore
import chaiHttp = require('chai-http');

import { app } from '../app';
import Example from '../database/models/ExampleModel';

import { Response } from 'superagent';

chai.use(chaiHttp);

const { expect } = chai;

describe('Seu teste', () => {
  /**
   * Exemplo do uso de stubs com tipos
   */

  let chaiHttpResponse: Response;

  before(async () => {
    sinon
      .stub(Example, "findOne")
      .resolves({
        ...<Seu mock>
      } as Example);
  });

  after(()=>{
    (Example.findOne as sinon.SinonStub).restore();
  })

  it('...', async () => {
    chaiHttpResponse = await chai
       .request(app)
       .get()
       // .post().send(<payload>)
       // .set(<field>, <value>) headers
       ...

    expect(...)
  });

  it('Seu sub-teste', () => {
    expect(false).to.be.eq(true);
  });
});
```
- /tests/unit
```ts
  it('Deveria lançar uma exceção quando a key é inválida', async function () {
    const paymentInput: IPayment = {
      payByPerson: 'Jobs',
      payToPerson: 'Wozniak',
      amount: 50000,
      key: '858.898.670-16XX',
    };

    sinon.stub(Model, 'create').resolves({});

    try {
      const service = new TransferService();
      await service.transfer(paymentInput);
    } catch (error) {
      expect((error as Error).message).to.be.equal('Invalid Key!');
    }
  });

  afterEach(function () {
    sinon.restore();
  });
```
- /database
  - models/index (sequelize)
```ts
// substitui o comando o arquivo gerado pelo comando `npx sequelize-cli --init:models`porém de forma bem mais simplificada
import { Sequelize } from 'sequelize';
import * as config from '../config/database';

const sequelize = new Sequelize(config)

export default sequelize;

```
- migration sequelize
```ts
'use strict';

module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.createTable('matches', {
      id: {
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
        type: Sequelize.INTEGER,
      },
      homeTeamId: {
        field: 'home_team_id',
        allowNull: false,
        type: Sequelize.INTEGER,
        references: {
          model: 'teams',
          key: 'id',
        },
        onDelete: 'CASCADE',
        onUpdate: 'CASCADE',
      },
      homeTeamGoals: {
        field: 'home_team_goals',
        allowNull: false,
        type: Sequelize.INTEGER,
      },
      awayTeamId: {
        field: 'away_team_id',
        allowNull: false,
        type: Sequelize.INTEGER,
        references: {
          model: 'teams',
          key: 'id',
        },
        onDelete: 'CASCADE',
        onUpdate: 'CASCADE',
      },
      awayTeamsGoals: {
        field: 'away_team_goals',
        allowNull: false,
        type: Sequelize.INTEGER,
      },
      inProgress: {
        field: 'in_progress',
        allowNull: false,
        type: Sequelize.BOOLEAN,
      }
    });
  },

  down: async (queryInterface, Sequelize) => {
    await queryInterface.dropTable('matches');
  }
};
```
- seeder sequelize
```ts
module.exports = {
  up: async (queryInterface) => {
    await queryInterface.bulkInsert('users', [
      {
        username: 'Admin',
        role: 'admin',
        email: 'admin@admin.com',
        password: '$2a$08$xi.Hxk1czAO0nZR..B393u10aED0RQ1N3PAEXQ7HxtLjKPEZBu.PW',
        // senha: 'secret_admin'
      },
      {
        username: 'User',
        role: 'user',
        email: 'user@user.com',
        password: '$2a$08$Y8Abi8jXvsXyqm.rmp0B.uQBA5qUz7T6Ghlg/CvVr/gLxYj5UAZVO',
        // senha: 'secret_user'
      },
    ], {});
  },

  down: async (queryInterface) => {
    await queryInterface.bulkDelete('users', null, {});
  },
};
```
