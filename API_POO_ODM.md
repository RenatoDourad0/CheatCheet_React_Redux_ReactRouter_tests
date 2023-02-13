# API com POO e ODM - express / mongoose

## DDD
- domain driven desing
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
  - definir do formato do domínio (interface)
  - criar classe do domínio
```ts
// src/Interfaces/IPayment.ts

interface IPayment {
  id?: string
  payByPerson: string;
  payToPerson: string;
  amount: number;
  key: string;
}

export default IPayment;

// src/Domain/Payment.ts

class Payment implements IPayment {
  private id: string | undefined; // undefined se os dados não estiverem no banco (Ex: antes do cadastro)
  private payByPerson: string;
  private payToPerson: string;
  private amount: number;
  private key: string;

  constructor(
    payByPerson: string,
    payToPerson: string,
    amount: number,
    key: string,
    id: string | undefined,
  ) {
    this.id = id;
    this.payByPerson = payByPerson;
    this.payToPerson = payToPerson;
    this.amount = amount;
    this.key = key;
  }

  public setId(id: string) {
    this.id = id;
  }

  public getId() {
    return this.id;
  }
  
  { ... }
  
  public setKey(key: string) {
    this.key = key;
  }

  public getKey() {
    return this.key;
  }
}

export default Payment;
```
  - elaborar o primeiro serviço baseado no domínio
```ts
Copiar
// src/Services/TransferService.ts

import Payment from '../Domain/Payment';

class TransferService {
  private isValidKey(key: string): boolean {
    const cpfRegex = /^\d{3}.\d{3}.\d{3}-\d{2}$/;
    return cpfRegex.test(key);
  }

  public async transfer(payment: IPayment) {
    if (!this.isValidKey(payment.key)) throw new Error('Invalid Key!');
    // Criar instância da Model de Payment usando Mongoose
    // Inserir os dados no banco
    // Retornar os dados com o id
  }
}

export default TransferService;
```
- criar model e schema para o domínio
```ts
// src/Models/PaymentODM.ts

import {
  Model,
  Schema,
  model,
  models,
} from 'mongoose';
import IPayment from '../Interfaces/IPayment';

class PaymentODM {
  private schema: Schema; // Atributo para o "molde"
  private model: Model<IPayment>; // Atributo para criar coleção e fornecer acesso ao banco

  constructor() {
    this.schema = new Schema<IPayment>({
      payByPerson: { type: String, required: true },
      payToPerson: { type: String, required: true },
      amount: { type: Number, required: true },
      key: { type: String, required: true },
    });
    this.model = models.Payment || model('Payment', this.schema); // Antes de criar o Schema, verificar se o schema já existe. Caso não exista, o schema será criado. 
  }

  public async create(payment: IPayment): Promise<IPayment> {
    return this.model.create({ ...payment });
  }
  
  public async update(id: string, obj: Partial<IPayment>):
  Promise<IPayment | null> {
    if (!isValidObjectId(id)) throw Error('Invalid Mongo id');
    
    return this.model.findByIdAndUpdate(
      { _id: id },
      { ...obj } as UpdateQuery<IPayment>,
      { new: true },
    );    
  }
}

export default PaymentODM;
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
- implementar o model na camada service
```ts
// src/Services/TransferService.ts

//import Payment from '../Domain/Payment';
import IPayment from '../Interfaces/IPayment';
import PaymentODM from '../Models/PaymentODM';

  // class TransferService {
    
  // private isValidKey(key: string): boolean {
  //   const cpfRegex = /^\d{3}.\d{3}.\d{3}-\d{2}$/;
  //   return cpfRegex.test(key);
  // }

    private createPaymentDomain(payment: IPayment | null): Payment | null {
    if (payment) {
      return new Payment(
        payment.payByPerson,
        payment.payToPerson,
        payment.amount,
        payment.key,
        payment.id,
      );
    }
    return null;
  }

  // public async transfer(payment: IPayment) {
    // if (!this.isValidKey(payment.key)) throw new Error('Invalid Key!');

    // Criar instância da Model de Payment usando Mongoose
    const paymentODM = new PaymentODM();
    // Inserir os dados no banco
    const newPayment = await paymentODM.create(payment);
    // Retornar os dados com o id
    return this.createPaymentDomain(newPayment);
  // }
  
  public async undoTransfer(id: string, payment: IPayment) {
    if (!this.isValidKey(payment.key)) throw new Error('Invalid Key!');
    const paymentODM = new PaymentODM();
    return paymentODM.update(id, payment);
  }
// }

// export default TransferService;
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
