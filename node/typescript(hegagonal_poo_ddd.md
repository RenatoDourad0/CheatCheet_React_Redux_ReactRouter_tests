# domain driven development + arquitetura hexagonal
- mentoria prática calaça
	- [dia 1](https://github.com/tryber/sd-023-b-live-lectures/tree/capacitacao/etapa_tecnica_1)
	- [dia 2](https://github.com/tryber/sd-023-b-live-lectures/tree/capacitacao/etapa_tecnica_2)

- domain consiste naquilo que é parte do core da aplicação e não depende de nenhuma tecnologia especifica. Entidades e regras de negócio são um exemplo
- arquitetura haxagonal é uma forma de se pensar o código atraves de portas e adaptadores. Sendo uma porta uma interface que determina um formato de comunicação/utilização e um adptador uma classe que implementa essa interface com suas especificidades
- repository é um padrão de projeto em que o acesso ao banco de dados é feito de forma desaclopada do codigo do core da aplicação. Permite que a tecnologia usada possa ser trocada sem grandes refatorações do código, somente mudando o parâmetro passado para a classe repositório de uma entidade 		- Para tal é criada para cada entidade da aplicação uma classe repository que tem como parametro de seu construtor uma interface que representa as operações com o banco. Essa classe repository é usada apenas para chamar as funções implementadas na classe passada por parametro (que respeitam a interface do construtor e implementam o código necessario para realizar a operação em uma tecnologia específica)
	- em domain/entities se cria uma entidade com suas propriedades e eventuais métodos
	- em seguida se cria um repository para a entidade aonde se faz as operações dela com o banco de dados atravéz da dependecia passada em seu construtor e do tipo criado em entities
	- no construtor do repository é feita uma inversão de dependecia com uma interface generica que representa a classe que implementa a tecnologia do banco de dados (persistence)(qualquer persistence que respeita essa interface pode ser usada no repository)
	- em persistence se cria uma classe que implementa a interface genérica citada acima para cada entidade da aplicação 
	

## organização do projeto
- app
	- src
		- application
			- server.ts
			- app.ts
		- domain
			- entities (classes que representam as entidades do software)
			- repository (são as operações que podem ser realizadas em uma entidade, semelhante ao model porém não envolve implementar a tecnologia para as operações)
			- useCases (semelhante a camada service)
		- infra
			- persistence (faz a conexão entre um repositório e uma tecnologia de banco de dados específica)
			- controllers
			- factories (instancia as camadas e injeta suas dependencias (controller / service / repository / persistence) para uso de uma entidade)
			- middlewares
				- errorMiddleware.ts 
			- routes
			- utils
				- conections
	- tests

## código
- exemplo do fluxo para criação de uma entidade
```ts
// src/domain/entities/user/User.ts

class User {
  id: number
  name: string
  email: string
  password: string

  constructor(user) {
    this.id = user.id;
    this.name =  user.name;
    this.email = user.email;
    this.password = user.password;
  }
}

export {User}


// src/domain/repository/user/IUserPersistance.ts

interface IUserPersistence { // definição da interface que representa as operações no banco de forma genérica
    register(entity: Omit<User, "id">): Promise<Pick<User, "name" | "email">>
    login(entity: Pick<User, "email" | "password">): Promise<string>
    findUserByEmail(email: string): Promise<User | undefined>
}

export {IUserPersistence}


// src/domain/repository/user/UserRepository.ts

import { User } from "../entities/User"
import { IUserPersistence } from "./IUserPersistence"

class UserRepository {
    constructor(private iPersistence: IUserPersistence) { } // inversão de dependencia com a interface que representa a persistence abstrata
    
    public register = async (entity: Omit<User, "id">): Promise<User> => { // usa a dependencia injetada para fazer as buscas de forma indireta
        return await this.iPersistence.register(entity)
    }

    public login = async(entity: Pick<User, "email" | "password">) => {
        return await this.iPersistence.login(entity)
    }

    public findUserByEmail = async(email: string) => {
        return this.iPersistence.findUserByEmail(email)
    }
}

export { UserRepository }


// src/domain/useCases/user/UserService.ts

import { User } from "../entities/User";
import { UserRepository } from "../repository/UserRepository";

class UserUseCase {

  constructor(private repository: UserRepository) {}

  public register = async (entity: Omit<User, "id">): Promise<User> => {
    if(!entity.email || !entity.name || !entity.password)
      throw new Error("Os campos não podem ser vazios");
    
    if(entity.email.length > 40)
      throw new Error("String email muito grande");

    if(await this.findUserByEmail(entity.email))
      throw new Error("Pessoa usuária já existe");
    
      return await this.repository.register(entity)
  }

  public login = async (entity: Omit<User, "id" | "name">): Promise<any> => {
    if(!entity.email || !entity.password)
      throw new Error("Os campos não podem ser vazios");
    
    return await this.repository.login(entity)
  }

  private findUserByEmail = async (email: string): Promise<User> => {
    return await this.repository.findUserByEmail(email)
  }
}

export default UserUseCase;


// src/infra/persistence/user/MySqlUserPersistence.ts

import { User } from '../../domain/entities/User';
import { IUserPersistence } from '../../domain/repository/IUserPersistence';
import db from '../utils/Connection';
import { ResultSetHeader } from "mysql2";
import jwt from 'jsonwebtoken';
import bcrypt from 'bcryptjs';

class MySqlUserPersistence implements IUserPersistence {

  public login = async (entity: Pick<User, "email" | "password">) => {
    const query = 'SELECT * FROM users WHERE email = ?';
    const values = [entity.email];

    const [data] = await db.execute(query, values);
    const [user] = data as User[]

    const isPasswordCorrect = bcrypt.compareSync(
      entity.password,
      user.password,
    );

    if (!isPasswordCorrect)
      throw new Error('Username ou password estão incorretos')

    const token = jwt.sign({ id: user.id }, 'jwtkey');

    return token
  }

  public findUserByEmail = async (email: string): Promise<User | null> => {
    const query = 'SELECT * FROM users WHERE email = ?';
    const values = [email];

    const [data] = await db.execute(query, values);
    const [user] = data as User[];

    return user || null;
  }

  public register = async (entity: User): Promise<Pick<User, "name" | "email">> => {
    const query = 'INSERT INTO Users (name, email, password) VALUES (?, ?, ?)';

    const salt = bcrypt.genSaltSync(10);
    const hash = bcrypt.hashSync(entity.password, salt);

    const values = [entity.name, entity.email, hash];
    await db.execute<ResultSetHeader>(query, values);

    const newUser: Pick<User, "name" | "email"> = { name: entity.name, email: entity.email};
    return newUser;
  }

}

export default MySqlUserPersistence;
```
- exemplo de infra da aplicação
