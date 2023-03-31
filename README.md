# Documentação da API

## Tabela de Conteúdos

- [Visão Geral](#1-visão-geral)
- [Início Rápido](#2-início-rápido)
  - [Instalando Dependências](#31-instalando-dependências)
  - [Variáveis de Ambiente](#22-variáveis-de-ambiente)
  - [Migrations](#23-migrations)
- [Endpoints](#4-endpoints)

---

## 1. Visão Geral

Visão geral do projeto, um pouco das tecnologias usadas.

- [NodeJS](https://nodejs.org/en/)
- [Express](https://expressjs.com/pt-br/)
- [TypeScript](https://www.typescriptlang.org/)
- [PostgreSQL](https://www.postgresql.org/)
- [TypeORM](https://typeorm.io/)
- [Yup](https://www.npmjs.com/package/yup)

## 2. Início Rápido

[ Voltar para o topo ](#tabela-de-conteúdos)

### 2.1. Instalando Dependências

Clone o projeto em sua máquina e instale as dependências com o comando:

```shell
yarn
```

### 2.2. Variáveis de Ambiente

Em seguida, crie um arquivo **.env**, copiando o formato do arquivo **.env.example**:

```
cp .env.example .env
```

Configure suas variáveis de ambiente com suas credenciais do Postgres e uma nova database da sua escolha.

### 2.3. Migrations

Execute as migrations com o comando:

```
yarn typeorm migration:run -d src/data-source.ts
```

## 3. Endpoints

[ Voltar para o topo ](#tabela-de-conteúdos)

### Índice

- [Clients](#1-clients)
  - [POST - /client](#11-criação-de-cliente)
  - [GET - /client](#12-listando-cliente-logado)
  - [DEL - /client/:client_id](#13-deleção-de-cliente-por-id)
- [Contacts](#2-contacts)

---

## 1. **Clients**

[ Voltar para os Endpoints ](#3-endpoints)

O objeto Client é definido como:

| Campo         | Tipo   | Descrição                        |
| ------------- | ------ | -------------------------------- |
| id            | string | Identificador único do usuário   |
| completedName | string | O nome do usuário.               |
| email         | string | O e-mail do usuário.             |
| password      | string | A senha de acesso do usuário     |
| cellphone     | string | O celular de cadastro do usuário |

### Endpoints

| Método | Rota             | Descrição                                      |
| ------ | ---------------- | ---------------------------------------------- |
| POST   | /client          | Criação de um usuário.                         |
| GET    | /client          | Lista todos os usuários                        |
| DEL    | /client/:user_id | Deleta um usuário usando seu ID como parâmetro |

---

### 1.1. **Criação de Clients**

[ Voltar para os Endpoints ](#5-endpoints)

### `/client`

### Exemplo de Request:

```
POST /client
Host: localhost:3000
Authorization: None
Content-type: application/json
```

### Corpo da Requisição:

```json
{
  "completeName": "clientTest",
  "cellphone": "(11) 99999-9999",
  "email": "clientTest@gmail.com",
  "password": "12345678"
}
```

### Schema de Validação com Yup:

```javascript
  cellphone: yup
    .string()
    .matches(
      /^\([1-9]{2}\) (?:[2-8]|9[1-9])[0-9]{3}\-[0-9]{4}$/,
      "Invalid cell phone"
    )
    .required(),
  completeName: yup.string().required(),
  email: yup.string().email().required(),
  password: yup
    .string()
    .min(8, "The password must have at least 8 characters")
    .required(),
  registerDate: yup.date().notRequired(),
```

OBS.: Chaves não presentes no schema serão removidas.

### Exemplo de Response:

```
201 Created
```

```json
{
{
	"email": "clientTest@gmail.com",
	"cellphone": "(11) 99999-9999",
	"completeName": "clientTest",
  "id": "a0bbe5dd-94da-4bc8-a346-31ba8ebcebee"
}
}
```

### Possíveis Erros:

| Código do Erro | Descrição                 |
| -------------- | ------------------------- |
| 409 Conflict   | Email already registered. |

---


Link da documentação completa : https://doc-api-full-stack-budeadzu6-gabrielgustavodias.vercel.app
