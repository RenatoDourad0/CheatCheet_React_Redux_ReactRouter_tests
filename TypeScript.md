# TypeScript

## Ambiente

### Instalação
- global
        - `npm install -g typescript@4.4`
- local
        - `npm i -D @tsconfig/node16@1.0 @types/node@16.11 typescript@4.4 ts-node-dev@1.1` 
			- instala o módulo typeScript e seus tipos, além do pacote de configuração do tsconfig.json para transpilar em node versão 16

### Inicialização e execução
- global
	- `tsc --init` para gerar arquivo tsconfig.json
	- executar com `tsc nomeDoArquivo.ts` que gera um arquivo .js que pode ser executadocom `node nomeDoArquivo.js`
- local
	- `npm init -y` 
	- `npx tsc --init` para gerar arquivo tsconfig.json
	- executar com `npx tsc nomeDoArquivo.ts` que gera um arquivo .js que pode ser executadocom `node nomeDoArquivo.js`
        
## Definições

### Types
 - boolean, number, string, enum, void, null, undefined e any
 - Promisse

### Type alias e interface
- funções similares para definição de tipos
- um nome para um tipo
- type alias
	- programação funcional
	- palavra reservada `type`
	- utiliza o operador de igualdade
	- pode herdar outro tipo ou interface através do `&`   
- interface
	- POO
	- palavra reservada `interface`    
	- não utiliza o operador de igualdade   
	- pode herdar outro tipo ou interface através da palavra reservada `extends`
```ts
type Tpoint = {
  x?: number, // propriedade opcional
  y: number
};
type Tcoordenates = {
	z: number
} & Tpoint
const position: Tcoordenates = { x: 1, y: 1, z: 1 };

interface Iemployee {
    firstName: string | number;
    lastName: string;
    fullName(): string;
};
interface Teacher extends Iemployee {
    subject?: string; // propriedade opcional
    sayHello(): string;
};
class Teacher {
	constructor(firstName: string | number, lastName: string, subject: string) {
		this.firstName = firstName;
		{...}
	}
}
```

### type assertion e generics       
- formas para usar os tipos 
- type assertion
	- forma de forçar um tipo para uma determinata estrutura durante o run time
	- restritivo, limita a execução a tipos específicos
	- representado pelo `:` ou pela palavra reservada `as` após a definição da estrutura
- generics
	- forma de passar por meio de parâmetros tipos para funções que se comportam de forma genérica
	- abrangente, permite a definição dos tipos na execução
	- representado pelo `<>` após a definição da estrutura
	- pedem ser implicitos. O transpilador determina o tipo com base nos inputs
```ts
// assertion
function stringToJson(str: string, id: number ): { name: string, id: number } {
  const result = JSON.parse(str);
  result.id = id;
  return result;
}
stringToJson('{ name: 'John' }', 01); // ok
stringToJson('{ name: 'John' }', '01'); // error

// generics
function stringToJson<T, U>(str: string, id: U ): T & { id: U } {
  const result = JSON.parse(str);
  result.id = id;
  return result;
}
type Person = {
        name: string
}
stringToJson<Person, number>('{ name: 'John' }', 01); // ok
stringToJson<Person, string>('{ name: 'John' }', '01'); // ok
stringToJson('{ name: 'John' }', '01'); // ok (retorno do tipo any)
```

### Type union e type intersection
- union
	- representada pelo símbulo `|` 
	- permite que uma entidade possa ter mais de um tipo      
- intersection
	- representada pelo símbulo `&`  
	- permite que o tipo de uma entidade seja a união de mais de um tipo
```ts
// type union
type Tdog = {
        id: number,
        name: string,
        age: number | string, // union
}
// ou
interface Idog {
        id: number,
        name: string,
        age: number | string,
}
const dogs: Tdog[] = [...];
function getDog(id: number): (Tdog | string) { // union
        return dogs.find(e => e.id === id) || 'not found';
}

// type intersection
type Tpupy = {
	isBrestFeeding: boolean
} & Tdog; // intersection
// ou
interface Ipupy extends Idog {
	isBrestFeeding: boolean
};
function getDog(id: number | string): Tpupy & { color: string } { // intersection
        const dog = dogs.find(e => e.id === id);
        dog.color = 'white';
        return dog;
}
```

## Uso

### MySql2
- a função execute tem seus tipos pré definidos por generics
- queries do tipo SELECT utilizam o tipo `RowDataPacket[]`, do tipo INSERT utilizam o `ResultSetHeader`
- deve ser feita uma intersesão entre os tipos nativos do execute e outro tipo que represente o objeto retornado pela query
```ts
// connection.ts
import mysql from 'mysql2/promise';
import 'dotenv/config';

export const connection = mysql.createPool({
  host: process.env.MYSQL_HOST,
  port: Number(process.env.MYSQL_PORT),
  user: process.env.MYSQL_ROOT_USER,
  password: process.env.MYSQL_ROOT_PASSWORD, 
  database: process.env.MYSQL_DATABASE,
  waitForConnections: true,
  connectionLimit: 1,
  queueLimit: 10,
});

// books.model.ts - funcional
import { RowDataPacket, ResultSetHeader, ... } from 'mysql2/promise';
{...}
type Book {
  id?: number;
  title: string;
  price: number;
  author: string;
  isbn: string;
};
// gelAll()
const [books] = await connection.execute<(RowDataPacket & Book)[]>('select * from books'); // array da interseção entre RowDataPacket e Book
// create()
const [{ insertId }] = await connection.execute<ResultSetHeader>(
  'INSERT INTO books (title, price, author, isbn) VALUES (?, ?, ?, ?)',
  [title, price, author, isbn]
);

// books.model.ts - POO
import { Pool, RowDataPacket } from 'mysql2/promise';
import connection from './connection';

interface Book {
  id?: number,
  title: string,
  price: number,
  author: string,
  isbn: string,
}
class BookModel {
  private connection: Pool;
  constructor(){
    this.connection = connection;
  }
  public async getAll(): Promise<Book[]> {
    const [rows] = await this.connection.execute<(Book & RowDataPacket)[]>(
      'SELECT * FROM books'
    );
    return rows;
  }
  public async create(book: Book): Promise<Book> {
    const { title, price, author, isbn } = book;
    const [{ insertId }] = await this.connection.execute<ResultSetHeader>(
        'INSERT INTO books (title, price, author, isbn) VALUES (?, ?, ?, ?)',
        [title, price, author, isbn]
    );
    return { id: insertId, ...book };
  }
}

// main.ts
import BookModel from './models/Book';
const main = async () => {
  const bookModel = new BookModel();
  const books = await bookModel.getAll();
  console.log(books);
};
main();
```
### Sequelize
- a implementação do Sequelize em uma aplicação com TypeScript difere um pouco do método que utiliza o sequelize-cli para criação de modelos. Isso porque o sequelize-cli não dá suporte nativo na interpretação/criação de migrations, seeders e models em TS
- altera `tsconfig.json`
	- `"rootDir": "./src",`
	- `"outDir": "./build",`
- instalar dependencias
	- `npm i dotenv@10.0 sequelize@6.3`
	- `npm i -D mysql2@2.3 sequelize-cli@6.2 @types/sequelize@4.28`
- gerar arquivo `.sequelizerc` na raiz
	- os caminhos de config e models-path apontam para pasta raiz build ao invés de src. Isso é necessário já que o CLI não deve conseguir interpretar esses recursos caso sejam em *.ts, sendo portanto necessária a transpilação desses recursos para JS Vanilla 
	- a pasta build é a pasta gerada após a transpilação da pasta src
	- Os arquivos em seeders-path e migrations-path não sofrerão ação do TS, dado que são em *.js. Portanto podem ficar em src
```ts
const path = require('path');

module.exports = {
  'config': path.resolve(__dirname, 'build', 'database', 'config', 'database.js'),
  'models-path': path.resolve(__dirname, 'build', 'database', 'models'),
  'seeders-path': path.resolve(__dirname, 'src', 'database', 'seeders'),
  'migrations-path': path.resolve(__dirname, 'src', 'database', 'migrations'),
};
```
- inicializar o sequelize-cli
	- `npx sequelize-cli init` 
- criar as pastas faltantes `./src/database/config/` e `./src/database/models/`
	- arquivos de configuração de modelos em TypeScript serão feitos manualmente
- criar o arquivo de configuração do banco
	- em `./src/database/config/database.ts`
	- o tipo Options disponibiliza as opções de configuração de banco do sequelize
	- a exportação do módulo deve ser feita utilizando somente export ao invés de export default ou export const, para que a transpilação o transforme em module.exports, o que é reconhecido pelo sequelize-cli
	- caso o objeto process acuse erro, incluir na aplicação o pacote @types/node como dependência de desenvolvimento
```ts
import 'dotenv/config';
import { Options } from 'sequelize';

const config: Options = {
  username: process.env.DB_USER || 'root',
  password: process.env.DB_PASS || '',
  database: process.env.DB_NAME || 'books_api',
  host: process.env.DB_HOST || 'localhost',
  port: Number(process.env.DB_PORT) || 3306,
  dialect: 'mysql',
};

export = config;
```
- criar o script `db:reset`
	- na chave scripts do `package.json`
	- transpila o código / dropa o banco se existir / cria o banco / cria as tabelas / popula as tabelas
	- `"db:reset": "npx -y tsc && npx sequelize-cli db:drop && npx sequelize-cli db:create && npx sequelize-cli db:migrate && npx sequelize-cli db:seed:all"`
- instanciar o sequelize
	- em `./src/database/models/index.ts`
	- criar uma instancia do sequelize que faz o mesmo papel do arquivo padrão gerado pelo sequeize-cli em build/models/index.js. Em outras palavras ativa o Sequelize para ser usado
```ts
import { Sequelize } from 'sequelize';
import * as config from '../config/database';

export default new Sequelize(config);
```
- a partir daqui, resta fazer manualmente os modelos em TS. As migrations e seeders podem ser feitas com ajuda do sequelize-cli e em js
- definindo os models
	- os modelos no Sequelize podem ser representados como classes que são a extensão (ou seja, que herdam atributos e métodos) da classe Model da mesma biblioteca. Para construirmos um modelo Sequelize em TypeScript devemos criar sua classe estendida, inicializá-la e depois exportá-la.
	- os tipos podem ser importados do próprio sequelize
	- como o modelo criado extende a classe Model do sequelize, as únicas propriedades que precisamos declarar nele são os seus atributos específicos. Entretanto, como eles são inicializados posteriormente por meio do método .init() do modelo criado, devemos afirmar que isso ocorrerá utilizando o prefixo declare (conforme a v6 do Sequelize)
	- diferentemente do sequelize-cli, que gera um modelo que usa a função sequelize.define dentro de uma constante para definir os campos, aqui você só precisa inicializar o modelo com .init()
	- adicionar dois campos especiais nos opcionais da definição do modelo (na segunda chave):
		- `sequelize`, que deve receber a instância construída no index.ts;
		- `modelName`, que deve fazer referência ao nome da tabela.
```ts
import { Model, INTEGER, STRING, DECIMAL } from 'sequelize';
import db from '.'; // o arquivo aonde o sequelize foi instanciado (./index.ts)

class Books extends Model {
  declare id: number;
  declare title: string;
}

Books.init({
  id: {
    type: INTEGER,
    allowNull: false,
    primaryKey: true,
    autoIncrement: true,
  },
  title: {
    type: STRING(30),
    allowNull: false,
  },
}, {
  sequelize: db,
  modelName: 'books',
  timestamps: false,
});

export default Books;
```
- associations
	- 1:N
		- as declarações de associations devem ficar concentradas em uma das entidades da relação 	
```ts
// ex. migration
module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.createTable('name', {
      id: {
        allowNull: false,
        autoIncrement: true,
        primaryKey: true,
        type: Sequelize.INTEGER,
      },
      { ... }
      bookId: {
        allowNull: false,
        type: Sequelize.INTEGER,
        references: {
          model: 'books',
          key: 'id',
        },
        field: 'book_id',
      },
    });
  },
  down: async (queryInterface) => {
    await queryInterface.dropTable('comments');
  },
};

// model genérico
import { Model } from 'sequelize';
import db from '.';
import OtherModel from './OtherModel'; // Nossa outra entidade

class Example extends Model {
  // declare <campo>: <tipo>;
}

Example.init({
  // ... Campos
}, {
  // ... Outras configs
  underscored: true,
  sequelize: db,
  // modelName: 'example',
  timestamps: false,
});

/**
  * `Workaround` para aplicar as associations em TS:
  * Associations 1:N devem ficar em uma das instâncias de modelo
  * */

OtherModel.belongsTo(Example, { foreignKey: 'campoA', as: 'campoEstrangeiroA' });
OtherModel.belongsTo(Example, { foreignKey: 'campoB', as: 'campoEstrangeiroB' });

Example.hasMany(OtherModel, { foreignKey: 'campoC', as: 'campoEstrangeiroC' });
Example.hasMany(OtherModel, { foreignKey: 'campoD', as: 'campoEstrangeiroD' });

export default Example;
```
### Express
#### Ambiente
##### instalação 
- `npm i -D @types/express@4.17`
- `npm i express@4.17 express-async-errors@3.1 restify-errors@8.0 @types/restify-errors@4.3`
- Adicionar ao package.json os scripts 
```json
    "start": "npm run build && node ./dist/index.js",
    "dev": "tsnd index.ts",
    "build": "tsc"
```

#### Inicialização e execução
- criar arquivo statusCodes.ts
```ts
// ./statusCodes.ts
const statusCodes = {
  OK: 200,
  NOT_FOUND: 404,
  CREATED: 201,
  BAD_REQUEST: 400,
  UNAUTHORIZED: 401,
  NO_CONTENT: 204,
};

export default statusCodes;
```
- criar arquivos app.ts e server.ts
```ts
// ./app.ts
import express, { NextFunction, Request, Response } from 'express';
import statusCodes from './statusCodes';
import 'express-async-errors';

const app = express();

app.use(express.json());

app.get('/', (_req: Request, res: Response) => {
  res.status(statusCodes.OK).send('Express + TypeScript');
});

app.use((err: Error, _req: Request, res: Response, next: NextFunction) => {
  const { name, message, details } = err as any;
  console.log(`name: ${name}`);

  switch (name) {
    case 'BadRequestError':
      res.status(400).json({ message });
      break;
    case 'ValidationError':
      res.status(400).json({ message: details[0].message });
      break;
    case 'NotFoundError':
      res.status(404).json({ message });
      break;
    case 'ConflictError':
      res.status(409).json({ message });
      break;
    default:
      console.error(err);
      res.sendStatus(500);
  }

  next();
});

export default app

// ./server.ts
import app from './app' 

const PORT = 8000;

app.listen(PORT, () => {
  console.log(`Server is running at http://localhost:${PORT}`);
});
```
#### CRUD - POO
- model (mysql2)
```ts
import { Pool, ResultSetHeader } from 'mysql2/promise';
import Book from '../interfaces/book.interface';

export default class BookModel {
  public connection: Pool;

  constructor(connection: Pool) {
    this.connection = connection;
  }

  public async getAll(): Promise<Book[]> {
    const result = await this.connection.execute('SELECT * FROM books');
    const [rows] = result;
    return rows as Book[];
  }

  public async create(book: Book): Promise<Book> {
    const { title, price, author, isbn } = book;
    const result = await this.connection.execute<ResultSetHeader>(
      'INSERT INTO books (title, price, author, isbn) VALUES (?, ?, ?, ?)',
      [title, price, author, isbn],
    );
    const [dataInserted] = result;
    const { insertId } = dataInserted;
    return { id: insertId, ...book };
  }
}
```
- model (sequelize)
```ts
import { Model, INTEGER, STRING } from 'sequelize';
import sequelize from '.'; 

export default class UserModel extends Model {
  declare id: number;
  declare name: string;
  declare email: string;
  declare password: string;
}

UserModel.init({
  id: {
    type: INTEGER,
    allowNull: false,
    primaryKey: true,
    autoIncrement: true,
  },
  name: {
    type: STRING(50),
    allowNull: false,
  },
  email: {
    type: STRING(50),
    allowNull: false,
    unique: true
  },
  password: {
    type: STRING(50),
    allowNull: false,
  }
}, {
  sequelize: sequelize,
  modelName: 'User',
  timestamps: false,
});
```
- service
```ts
import UserModel from '../database/models/user.model';
import User from '../interfaces/user.interface';

export default class UserService {
  user;
  constructor() {
    this.user = UserModel
  }
  
  public async getAll(): Promise<User[]> {
    const users = await this.user.findAll();
    return users
  }
}
```
- controller
```ts
import { Request, Response } from 'express'
import User from '../interfaces/user.interface';
import UserService from '../services/user.service';
import statusCodes from '../helpers/statusCode';

export default class UserController {
  user;
  constructor() {
    this.user = new UserService();
  }

  getUsers = async (req: Request, res: Response ): Promise<Response<{users: User[]}>> => {
    const users = await this.user.getAll();
    return res.status(statusCodes.OK).json({ users })
  }
}
```
- route
```ts
import { Router } from 'express'
import UserController from '../controllers/user.controller';

const userRoute = Router();
const userController = new UserController();

userRoute.get('/', userController.getUsers)

export = userRoute;
```
