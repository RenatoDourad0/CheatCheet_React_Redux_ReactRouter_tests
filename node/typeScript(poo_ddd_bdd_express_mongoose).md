# API com POO e ODM - express / mongoose

## DDD
- domain driven desing
- o objetivo e desaclobar o código do core da aplicação de qualuqer tecnologia 
- a primeira etapa na construção de aplicações orientadas a objetos é determinar quais são os objetos da regra de negócio, os seus atributos e suas funcionalidades (os domínios que a aplicação deve abranger). As primeiras classes da aplicação devem ser definidas sob o olhar das regras de negócio. Em outras palavras, não devemos iniciar a aplicação pensando nas classes do banco de dados ou de alguma tecnologia.
- uma vez que as classes do domínio foram identificadas e desenvolvidas, pode-se seguir para as próximas etapas da construção. Ainda sob o olhar da regra de negócio, deve se iniciar a construção do serviço e construir as demais camadas à medida que forem necessárias para a nossa aplicação
- as classes inicalmente não devem ter dependencias nem interfaces, sendo adicionadas a medida que necessário

## BDD
- behavior driven development
- prática a ser usada junto com TDD que preza por organizar os casos de teste por ações nos domínios da aplicação
- testa o service. Controller e model devem ser mokados

## mongoose
- [Docs](https://mongoosejs.com/docs/guide.html)
- ODM = object document mapping
- consiste em uma implementação de ODM para aplicações desenvolvidas em Node.Js, com vistas a remover a complexidade na interação com o MongoDB. Para isso, são definidos Schemas e Models para cada Collection no banco de dados
- exemplo
  - teste unitário do serviço de 'chave'
```ts
import { expect } from 'chai';
import sinon from 'sinon';
import { Model } from 'mongoose';
import Key from '../../../src/Domain/Key/Key';
import IKey from '../../../src/Interfaces/IKey';
import KeyService from '../../../src/Services/KeyService';

const RESULT_ERROR = 'Invalid Key';

describe('Deveria validar e criar chaves', function () {
  it('Criando uma chave de tipo CPF com SUCESSO', async function () {
    const keyInput: IKey = {
      value: '478.966.190-32',
      owner: 'Jack C.',
      type: 'cpf',
    };
    const keyOutput: Key = new Key(
      '478.966.190-32',
      'Jack C.',
      'cpf',
      '633ec9fa3df977e30e993492',
    );
    sinon.stub(Model, 'create').resolves(keyOutput);

    const service = new KeyService();
    const result = await service.register(keyInput);

    expect(result).to.be.deep.equal(keyOutput);
  });

  it('Criando uma chave de tipo CPF inválida', async function () {
    const keyInput: IKey = {
      value: '478.966.190-32XX',
      owner: 'Jack C.',
      type: 'cpf',
    };
    sinon.stub(Model, 'create').resolves({});
    
    try {
      const service = new KeyService();
      await service.register(keyInput);
    } catch (error) {
      expect((error as Error).message).to.be.equal(RESULT_ERROR);
    }
  });
});
```
  - definir o formato do domínio (interface) - formato de entrada dos dados da entidade (ex: não inclui id)
  - criar classes do domínio - formato de saída/manipulação da entidade dentro da aplicação (ex: com id e acesso a todos os atributos)
```ts
// src/Interfaces/IKey.ts

interface IKey { // representa o formato da abstração 'chave'
  id?: string;
  value: string;
  owner: string;
  type: string;
}

export default IKey;

// src/Interfaces/IValid.ts

interface IValid { // representa uma propriedade que 'chaves' concretas devem ter mas que não existe na abstração base 'chave'
  isValid(value: string): boolean;
}
 
export default IValid;

// src/utils/KeyTypes.ts

enum KeyTypes { // enum que representa um mapa dos valores da propriedade tipo da chave
  CPF = 'cpf',
  PHONE_NUMBER = 'phonenumber',
  MAIL = 'mail',
}

export default KeyTypes;

// src/Domain/Key/Key.ts

class Key { // representa a abstração 'chave' genérica com acesso a todos os métodos e atributos
  private value: string;
  private owner: string;
  private type: string;
  private id: string | undefined;

  constructor(
    value: string,
    owner: string,
    type: string,
    id: string | undefined,
  ) {
    this.value = value;
    this.owner = owner;
    this.type = type;
    this.id = id;
  }

  public getValue() {
    return this.value;
  }

  public setValue(value: string) {
    this.value = value;
  }
  
  { ... }
  
  public getType() {
    return this.type;
  }

  public setType(type: string) {
    this.type = type;
  }
}

export default Key;

// src/Domain/Key/CPF.ts

class CPF implements IKey, IValid { //representa uma 'chave' concreta
  readonly value: string;
  readonly owner: string;
  readonly type: string;

  constructor(value: string, owner: string) {
    if (!this.isValid(value)) throw Error('Invalid Key');
    this.value = value;
    this.owner = owner;
    this.type = KeyTypes.CPF; // define o tipo como cpf para todas as entidades dessa classe
  }

  isValid(value: string): boolean {
    const regex = /^\d{3}\.\d{3}\.\d{3}-\d{2}$/;
    return regex.test(value);
  }
}

export default CPF;

// src/Domain/Key/KeyFactory.ts

class KeyFactory { // desing pattern para criação de instancias de classes diferentes que compartilham da mesma estrutura
  public static create(key: IKey): IKey & IValid { // classes diferentes são instanciadas mas todas retornam o mesmo tipo 
    if (key.type === KeyTypes.CPF) {
      return new CPF(key.value, key.owner);
    }
    if (key.type === KeyTypes.PHONE_NUMBER) {
      return new PhoneNumber(key.value, key.owner);
    }
    if (key.type === KeyTypes.MAIL) {
      return new Mail(key.value, key.owner);
    }
    throw new Error('Invalid Key Type!');
  }
}

export default KeyFactory;
```
  - elaborar o primeiro serviço baseado no domínio
```ts
// src/Services/KeyService.ts

class KeyService {
  private createKeyDomain(key: IKey | null): Key | null { // retorna 'chave' no tipo definido na classe key (que é o tipo genérico usado por todas as 'chaves' concretas. Ex: cpf, telefone e email compartilham da estrutura da classe Key)
    if (key) {
      return new Key(
        key.value,
        key.owner,
        key.type,
        key.id,
      ); 
    }
    return null;
  }

  public async register(data: IKey) {
    const typedKey = KeyFactory.create(data); // retona 'chave' como entidade da classe concreta que a representa
    const keyODM = new KeyODM(); // instancia o model
    const newkey = await keyODM.create(typedKey); // persiste a 'chave' na db
    return this.createKeyDomain(newkey); // retorna a nova 'chave' com base na interface genérica Key
  }

  public async getByValue(value: string) {
    const keyODM = new KeyODM();
    const key = await keyODM.findByValue(value);
    return this.createKeyDomain(key);
  }
}

export default KeyService;
```
- criar model e schema para o domínio
```ts
// src/Models/AbstractODM.ts

import {
  isValidObjectId,
  Model,
  models,
  Schema,
  UpdateQuery,
  model,
} from 'mongoose';

abstract class AbstractODM<T> { // representa um model genérico e abstrado para interagir com o banco mongodb
  protected model: Model<T>;  // o model do mongoose é criado com base na interface passada para o generico
  protected schema: Schema;
  protected modelName: string;

  constructor(schema: Schema, modelName: string) {
    this.schema = schema;
    this.modelName = modelName;
    this.model = models[this.modelName] || model(this.modelName, this.schema); // aplica desing patter Singleton para garantir somente uma instancia do model possa existir (necessario para funcionamento do mongoose)
  }

  public async create(obj: T): Promise<T> {
    return this.model.create({ ...obj });
  }

  public async update(_id: string, obj: Partial<T>): Promise<T | null> {
    if (!isValidObjectId(_id)) throw Error('Invalid Mongo id');

    return this.model.findByIdAndUpdate(
      { _id },
      { ...obj } as UpdateQuery<T>,
      { new: true },
    );
  }
}

export default AbstractODM;

// src/Models/KeyODM.ts

import { Schema } from 'mongoose';
import IKey from '../Interfaces/IKey';
import AbstractODM from './AbstractODM';

class KeyODM extends AbstractODM<IKey> { // define o genérico como a interface que representa o domínio
  constructor() {
    const schema = new Schema<IKey>({ // cria um schema do mongoose
      value: { type: String, required: true },
      owner: { type: String, required: true },
      type: { type: String, required: true },
    });
    super(schema, 'Key'); // chama a classe abstrada que define o model que será utilizado 
  }

  public async findByValue(value: string): Promise<IKey | null> {
    return this.model.findOne({ value });
  }
}

export default KeyODM;
```

- conexão ao servidor mongo
```ts
// src/Models/Connection.ts

import mongoose from 'mongoose';
import 'dotenv/config';

const MONGO_DB_URL = 'mongodb://localhost:27017/trix';

const connectToDatabase = (
  mongoDatabaseURI = process.env.MONGO_URI
    || MONGO_DB_URL,
) => mongoose.connect(mongoDatabaseURI);

export default connectToDatabase;
```

- contruindo o controller
```ts
import { NextFunction, Request, Response } from 'express';
import IPayment from '../Interfaces/IPayment';
import TransferService from '../Services/TransferService';

class TransferController {
  private req: Request;
  private res: Response;
  private next: NextFunction;
  private service: TransferService;

  constructor(req: Request, res: Response, next: NextFunction) {
    this.req = req;
    this.res = res;
    this.next = next;
    this.service = new TransferService();
  }

  public async create() {
    const payment: IPayment = {
      payByPerson: this.req.body.payByPerson,
      payToPerson: this.req.body.payToPerson,
      amount: this.req.body.amount,
      key: this.req.body.key,
    };

    try {
      const newPayment = await this.service.transfer(payment);
      return this.res.status(201).json(newPayment);
    } catch (error) {
      this.next(error);
    }
  }
  
  public async reversalRequest() {
    const payment: IPayment = {
      ...this.req.body,
      status: PaymentStatus.reversed,
    };
    const { id } = this.req.params;
    try {
      await this.service.undoTransfer(id, payment);
      return this.res.status(204).json({});
    } catch (error) {
      this.next(error);
    }
  }
 }
  
export default TransferController;
```

- middleware de erro
```ts
// src/Middlewares/ErrorHandler.ts

import { NextFunction, Request, Response } from 'express';

class ErrorHandler {
  public static handle(
    error: Error,
    _req: Request,
    res: Response,
    next: NextFunction,
  ) {
    res.status(500).json({ message: error.message });
    next();
  }
}

export default ErrorHandler;
```

- definição de rotas
```ts
// src/Routes/Routes.ts

import { Router } from 'express';
import TransferController from '../Controllers/TransferController';

const routes = Router();

routes.post(
  '/transfer',
  (req, res, next) => new TransferController(req, res, next).create(),
);

routes.patch(
  '/transfer/:id',
  (req, res, next) => new TransferController(req, res, next).reversalRequest(),
);

export default routes;
```

- app.ts
```ts
// src/app.ts

import express from 'express';
import ErrorHandler from './Middlewares/ErrorHandler';
import routes from './Routes/Routes';

const app = express();
app.use(express.json());
app.use(routes);
app.use(ErrorHandler.handle);

export default app;
```

- server.ts
```ts
// src/server.ts

import 'dotenv/config';
import app from './app';
import connectToDatabase from './Models/Connection';

const PORT = process.env.PORT || 3001;
connectToDatabase()
  .then(() => {
    app.listen(PORT, () => console.log(`Running server on port: ${PORT}`));
  })
  .catch((error) => {
    console.log('Connection with database generated an error:\r\n');
    console.error(error);
    console.log('\r\nServer initialization cancelled');
    process.exit(0);
  });
```
