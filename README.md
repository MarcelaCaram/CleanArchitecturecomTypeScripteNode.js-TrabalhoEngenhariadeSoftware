# Trabalho 1 - Engenharia de Software

# Roteiro Prático - Clean Architecture com TypeScript e Node.js

### Alunos:

- Lara Gama Santos (20213001497)
- Lucas Santos Rodrigues (20213001521)
- Marcela Camarano Caram Peito (20213006528)
- Vinicius Ferreira Pinheiro (20213005208)

## O que é Clean Architecture?
  Clean Architecture é uma arquitetura de software que tem por objetivo padronizar e organizar o código desenvolvido, favorecendo a sua reusabilidade e a independência de tecnologia. Dessa forma, a lógica de negócios é encapsulada e separada do mecanismo de entrega. Entre algumas vantagens da Clean Architecture estão:

- Facilita a realização de testes
- Permite independência da interface do usuário
- Permite independência do banco de dados
- Permite independência de agentes externos, como Frameworks

  A Clean Architecture promove a separação de responsabilidades em um sistema, dividindo-o em várias camadas ou níveis, conforme pode ser observado na imagem abaixo. Dentre as camadas representadas estão: entidades (classes comuns a vários sistemas), casos de uso (regras de negócio específicas do sistema), adaptadores (viabilizam a comunicação entre as camadas internas e a camada externa) e frameworks externos (bibliotecas, frameworks e sistemas externos).

<div align="center">
    <img src="cleanArch.png">
</div>

## Como fazer uma Clean Architecture com TypeScript e Node.js?
  Ao combinar os princípios da Clean Architecture com as capacidades de tipagem estática do TypeScript e a versatilidade do ambiente de execução do Node.js, os desenvolvedores podem criar aplicativos escaláveis e de alta qualidade. Essa abordagem pode ser feita por meio do cumprimento dos seguintes tópicos chaves:

- `Entidades`: Defina objetos de negócios centrais, como 'Usuário', 'Produto', 'Pedido', etc.
- `Casos de Uso` (Use Cases): Desenvolva métodos principais, como 'Criar Usuário', 'Atualizar Produto', 'Processar Pedido', etc.
- `Interfaces de Controlador`: Crie interfaces de controlador para traduzir solicitações HTTP em chamadas de casos de uso, como 'UserController', 'ProductController', etc.
- `Camada de Aplicativo`: Implemente a lógica intermediária que coordena interações entre entidades e casos de uso.
- `Camada de Infraestrutura`: Gerencie conexões com bibliotecas externas, bancos de dados e serviços de terceiros, como repositórios e adaptadores de banco de dados.

## Instalação das ferramentas
  Para este tutorial, será necessária a instalação do Node.js e do Typescript.

#### Instalação do Node.js 
  Realize a instalação do Node.js através do seguinte link: 
https://nodejs.org/en/download/

#### Verificando a versão instalada
```shellscript
node --version
```

#### Iniciando uma aplicação
```shellscript
mkdir nodeapp 
```

```shellscript
cd nodeapp  
``` 

```shellscript
npm init
```

### Instalação do Typescript
  Após a instalação do NodeJs vamos criar um projeto chamado **ts** e aplicar o comando no terminal dentro do diretório do projeto
```shellscript
npm init
```
Este comando criará o arquivo **package.json**, que será útil posteriormente. Aperte enter para pular todas as perguntas que surgirão após o comando.

#### Para instalar o TypeScript, basta aplicar o comando:
```shellscript
npm install typescript
```
  Perceba que o arquivo **package.json** já exibe o pacote do TypeScript como dependência.
  Agora vamos iniciar as configurações do TypeScript. Primeiro, aplique o comando:

```shellscript
npx tsc --init
```
Este comando irá gerar o arquivo **tsconfig.json**, que será responsável pelas configurações do TypeScript.
Neste arquivo, devemos alterar o parâmetro **"modules": "commonjs"** para **"modules": "ESNext"**, aqui estamos avisando ao TypeScript para usar o padrão do EcmaScript mais atual. Também vamos deixar ativado o parâmetro **"outDir": "./dist"**, que será o diretório onde o TypeScript irá gerar o código JavaScript, lembrando que o TypeScript gera um código JavaScript.

### Criando primeiro código em TypeScript
  Para testar a instalação acima, vamos criar um arquivo teste.ts na raiz de nosso projeto com seguinte código:
```typescript
let idade: number = 20;
let nome: string = 'Vinicius';

console.log(`nome: ${nome}, idade: ${idade}`);
```
  TypeScript nesse momento, a ideia neste exemplo é verificar se nosso ambiente está ok, para isso vamos executar o seguinte comando:
```shellscript
npx tsc
```
  Podemos testar e executar o código JavaScript gerado, vamos usar o comando:
```shellscript
node ./dist/teste.js
```
Caso esteja correto, o resultado do comando acima deve ser:
```shellscript
nome: Vinicius, idade: 20
```
### Criando um código Clean Architecture com TypeScript e Node.js
  Como exemplo de aplicação usando Clean Architecture, será criada uma lista de tarefas a fazer (task list).

  Primeiramente, vamos criar um novo projeto Node.js e inicializar o npm:
  
```bash
mkdir clean-architecture-demo
cd clean-architecture-demo
npm init -y
```

#### Instalando Dependências

Depois, instale as dependências necessárias (Inversify, inversify-express-utils e Express):

```bash
npm install inversify inversify-express-utils express reflect-metadata
npm install @types/express --save-dev
npm install typescript ts-node --save-dev
npm install
```

####  Configurando o TypeScript

Crie um arquivo `tsconfig.json` na raiz do projeto e adicione as seguintes configurações:

```json
{
  "compilerOptions": {
    "target": "es6",
    "module": "commonjs",
    "outDir": "dist",
    "strict": true,
    "esModuleInterop": true,
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true
  }
}
```

####  Implementando as Camadas da Clean Architecture

#####  - Entidades

Crie uma pasta chamada `entities` e adicione um arquivo `Task.ts`:

```typescript
export class Task {
    constructor(public id: number, public description: string, public status: string) {}
}
```

##### - Casos de Uso

Crie uma pasta chamada `useCases` e adicione um arquivo `TaskListUseCase.ts`:

```typescript
"use strict";
var __decorate = (this && this.__decorate) || function (decorators, target, key, desc) {
    var c = arguments.length, r = c < 3 ? target : desc === null ? desc = Object.getOwnPropertyDescriptor(target, key) : desc, d;
    if (typeof Reflect === "object" && typeof Reflect.decorate === "function") r = Reflect.decorate(decorators, target, key, desc);
    else for (var i = decorators.length - 1; i >= 0; i--) if (d = decorators[i]) r = (c < 3 ? d(r) : c > 3 ? d(target, key, r) : d(target, key)) || r;
    return c > 3 && r && Object.defineProperty(target, key, r), r;
};
Object.defineProperty(exports, "__esModule", { value: true });
exports.TaskListUseCase = void 0;
const inversify_1 = require("inversify");
const Task_1 = require("../entities/Task");
let TaskListUseCase = class TaskListUseCase {
    getTasks() {
        return [
            new Task_1.Task(1, "Doing the dishes", "Done"),
            new Task_1.Task(2, "Laying the", "Done"),
            new Task_1.Task(3, "Cleaning the bathroom", "Undone"),
            new Task_1.Task(4, "Cooking dinner", "Undone"),
            new Task_1.Task(5, "Ironing clothes", "Undone"),
        ];
    }
};
exports.TaskListUseCase = TaskListUseCase;
exports.TaskListUseCase = TaskListUseCase = __decorate([
    (0, inversify_1.injectable)()
], TaskListUseCase);

```

##### - Controladores

Crie uma pasta chamada `controllers` e adicione um arquivo `TaskController.ts`:

```typescript
"use strict";
var __decorate = (this && this.__decorate) || function (decorators, target, key, desc) {
    var c = arguments.length, r = c < 3 ? target : desc === null ? desc = Object.getOwnPropertyDescriptor(target, key) : desc, d;
    if (typeof Reflect === "object" && typeof Reflect.decorate === "function") r = Reflect.decorate(decorators, target, key, desc);
    else for (var i = decorators.length - 1; i >= 0; i--) if (d = decorators[i]) r = (c < 3 ? d(r) : c > 3 ? d(target, key, r) : d(target, key)) || r;
    return c > 3 && r && Object.defineProperty(target, key, r), r;
};
var __metadata = (this && this.__metadata) || function (k, v) {
    if (typeof Reflect === "object" && typeof Reflect.metadata === "function") return Reflect.metadata(k, v);
};
var __param = (this && this.__param) || function (paramIndex, decorator) {
    return function (target, key) { decorator(target, key, paramIndex); }
};
var __awaiter = (this && this.__awaiter) || function (thisArg, _arguments, P, generator) {
    function adopt(value) { return value instanceof P ? value : new P(function (resolve) { resolve(value); }); }
    return new (P || (P = Promise))(function (resolve, reject) {
        function fulfilled(value) { try { step(generator.next(value)); } catch (e) { reject(e); } }
        function rejected(value) { try { step(generator["throw"](value)); } catch (e) { reject(e); } }
        function step(result) { result.done ? resolve(result.value) : adopt(result.value).then(fulfilled, rejected); }
        step((generator = generator.apply(thisArg, _arguments || [])).next());
    });
};
Object.defineProperty(exports, "__esModule", { value: true });
exports.TaskController = void 0;
const inversify_express_utils_1 = require("inversify-express-utils");
const TaskListUseCase_1 = require("../useCases/TaskListUseCase");
const inversify_1 = require("inversify");
let TaskController = class TaskController {
    constructor(taskListUseCase) {
        this.taskListUseCase = taskListUseCase;
    }
    getUsers(_, res) {
        return __awaiter(this, void 0, void 0, function* () {
            const users = this.taskListUseCase.getTasks();
            return res.json(users);
        });
    }
};
exports.TaskController = TaskController;
__decorate([
    (0, inversify_express_utils_1.httpGet)("/"),
    __metadata("design:type", Function),
    __metadata("design:paramtypes", [Object, Object]),
    __metadata("design:returntype", Promise)
], TaskController.prototype, "getUsers", null);
exports.TaskController = TaskController = __decorate([
    (0, inversify_express_utils_1.controller)("/tasks"),
    __param(0, (0, inversify_1.inject)(TaskListUseCase_1.TaskListUseCase)),
    __metadata("design:paramtypes", [TaskListUseCase_1.TaskListUseCase])
], TaskController);

```

#### Configurando o Contêiner Inversify

Crie um arquivo chamado `inversify.config.ts`:

```typescript
import { Container } from "inversify";
import { TaskController } from "./controllers/TaskController";
import { TaskListUseCase } from "./useCases/TaskListUseCase";

const container = new Container();
container.bind<TaskController>(TaskController).toSelf();
container.bind<TaskListUseCase>(TaskListUseCase).toSelf();

export default container;

```

#### Configurando o Servidor Express

Crie um arquivo chamado `main.ts`:

```typescript
import "reflect-metadata";
import { InversifyExpressServer } from "inversify-express-utils";
import container from "./inversify.config";

const server = new InversifyExpressServer(container);

server.build().listen(3000, () => {
  console.log("Servidor iniciado em http://localhost:3000");
});
```
#### Executando o projeto

Para executar o projeto, utilize os seguintes comandos:
```shellscript
npx tsc
node ./dist/tasks.js
```

#### Abrindo o localhost

Para acessar o resultado, acesse na url de seu navegador:
http://localhost:3000/tasks/

#### Resultado

Deve ser exibida, no localhost, a seguinte lista de tarefas a fazer:
![image](https://github.com/MarcelaCaram/CleanArchitecturecomTypeScripteNode.js-TrabalhoEngenhariadeSoftware/assets/81043748/c6c0a3a7-3c3f-4fe9-9865-fa643fe3e72d)











