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
	- fornece sua tipagem a classe de mesmo nome que a interface
```ts
type Tpoint = {
  x?: number, // propriedade opcional
  y: number
};

type Tcoordenates = {
	z: number
} & Tpoint

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
		...
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

### Sequelize

### Express
