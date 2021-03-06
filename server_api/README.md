# Ecoleta RocketSeat Server

## Install NodeJS on Windows 

Run `Get-ExecutionPolicy`. If it returns `Restricted`, then run `Set-ExecutionPolicy AllSigned` or `Set-ExecutionPolicy Bypass -Scope Process`.

<span style="color:black">$></span> ` Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))`

<span style="color:black">$></span> `cinst nodejs-lts`

> NodeJs comes with npm, which does not need to be installed separately

## Creating Project Base

<span style="color:black">$></span> `mkdir server`  

<span style="color:black">$></span> `cd server`

<span style="color:black">$></span> `npm init -y`

<span style="color:black">$></span> `code .`

<span style="color:black">$></span> `npm install express`

>  create `src` folder

>  create `server.ts` file on src folder

<span style="color:black">$></span> `npm install @types/express -D`

<span style="color:black">$></span> `npm install ts-node -D`

<span style="color:black">$></span> `npm install ts-node-dev -D`

<span style="color:black">$></span> `npm install typescript -D`

<span style="color:black">$></span> `npx tsc --init`
 
<span style="color:black">$></span> `npx ts-node src/server.ts`

> App is running at this point 

Open package.json and add dev script command

```javascript
...
"scripts": {
    "dev": "ts-node-dev src/server.ts"
  }
...
```

<span style="color:black">$></span> `npm run dev`


## ORM e Banco de dados utilizados

<span style="color:black">$></span> `npm install sqlite3`

ou 

<span style="color:black">$></span> `npm install pg`

- Knex.js

<span style="color:black">$></span> `npm install knex`

Exemplo de uso em consulta:

```javascript
Knex('users').where('name', 'Marcos').select('*')
```

run migrations 

<span style="color:black">$></span> `npx knex migrate:latest --knexfile knexfile.ts migrate:latest`

ou 

```javascript
"scripts": {
    "dev": "ts-node-dev src/server.ts",
    "knex:migrate" : "knex migrate:latest --knexfile knexfile.ts migrate:latest"
  }
```

e rodar com 

<span style="color:black">$></span> `npm run knex:migrate`

## Anotações

```javascript
app.use(express.json());
``` 
Adicionado para que o express entenda as requisições no formato JSON

#### Rotas x Recursos

rota: url completa da requisição 

Recurso: qual entidade estamos acessando no sistema

#### Exemplos de métodos HTTP

GET: Buscar uma ou mais informações no back-end

POST: Criar uma nova informação no back-end

PUT: Atualizar informação no back-end

DELETE: Remover informação no back-end

Exemplos:
> POST: http://localhost:3333/users = Criar um usuário

> GET: http://localhost:3333/users = buscar usuários

> GET: http://localhost:3333/users/1 = buscar um único usuário

#### Tipos de parâmetros

Request Param: Parâmetro adicionado na própria rota e que identifica um recurso. 
> http://localhost:3333/users/1

Query param: Parâmetros opcionais adicionados na rota. 
> http://localhost:3333/users?search=os

Request body: Corpo da requisição


### Anotações Código 


```javascript
import express from 'express';

const app = express();

app.use(express.json());

const users = [
    'Marcos', // 0
    'João',   // 1
    'Celso',  // 2
    'Daniel', // 3
    'Carlos'  // 4
];

app.get('/users', (request, response) => {
    console.log('User list');

    const search = request.query.search;

    const filteredUsers = search ? users.filter(user => user.toLowerCase().includes(String(search).toLowerCase())) : users;

    // JSON
    return response.json(filteredUsers);

});

app.get('/users/:id', (request, response) => {
    console.log('User list');

    // JSON
    return response.json(users[Number(request.params.id)]);

});


app.post('/users', (request, response) => {

    const data = request.body;

    const user = {
        name: data.name,
        email: data.email
    };

    //users.push(request.body.name)
    users.push(user.name)

    return response.json(user);
})

app.listen(3333);
```