# RudBee — Guia de Criação, Validação e Login de Usuários no RudBee

Este guia mostra como configurar a criação, validação e login de um novo usuário local no RudBee.

---

Após realizar a etapa de instalação das dependências necessárias e configuração das mesmas, devemos, localmente, realizar um passo manual na criação, validação e login de um novo usuário dentro do back-end do RudBee. Para isso, será necessário um software específico para realizar requisições, como [Postman](https://www.postman.com/downloads/) ou [Insomnia](https://insomnia.rest/download), instale/use qual for de sua preferência. 

---

## Criação, Validação e Login de Usuário 

Siga os passos abaixo para realizar as ações necessárias:

### 1. Criar Usuário no RudBee

Endpoint: **POST /users**
Descrição: Cria um novo usuário no sistema.

Exemplo de corpo da requisição:

```bash
{
    "login": "<SEU_LOGIN>",
    "password": "<SUA_SENHA>",
    "name": "<SEU_NOME>",
    "active": true,
    "basePath": "teste",
    "perfil": "OWNER"
}
```

### 2. Validar Usuário np RudBee

Endpoint: **POST /rudbee_users/reg**
Descrição: Valida os dados de um usuário previamente registrado.

Exemplo de corpo da requisição:

```bash
{
    "active": true,
    "change_password_code": "<CÓDIGO_DE_REDEFINIÇÃO>",
    "change_password_expiration": "2025-08-28T21:09:22.080Z",
    "email": "<EMAIL_DO_USUÁRIO>",
    "first_name": "<PRIMEIRO_NOME>",
    "isadm": false,
    "last_name": "<SOBRENOME>",
    "password": "<SENHA_CRIPTOGRAFADA>",
    "permissions": {
        "member": {},
        "owner": {}
    },
    "photo": null,
    "preferences": {
        "locale": "pt-BR"
    },
    "role": "ADMIN",
    "salt": "<SALT_BCRYPT>",
    "uid": "<ID_UNIVERSAL_DO_USUÁRIO>",
    "username": "<NOME_DE_USUÁRIO>"
}
```

### 3. Login do Usuário no RudBee

Endpoint: **POST /auth/login**
Descrição: Realiza o login de um usuário no sistema.

Exemplo de corpo da requisição:

```bash
{
    "login": "<SEU_LOGIN>",
    "password": "<SUA_SENHA>"
}
```

🔐 Após um login bem-sucedido, a API deve retornar um token JWT ou sessão, que deve ser usado nas requisições futuras com autenticação.