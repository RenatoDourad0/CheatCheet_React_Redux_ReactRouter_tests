# TypeScript

## Ambiente

### Instalação
- global
        - `npm install -g typescript@4.4`
- local
        - `npm i -D @tsconfig/node16@1.0 @types/node@16.11 typescript@4.4 ts-node` 
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

### Express
