# RudBee — Guia de Instalação

Este guia mostra como instalar e iniciar o projeto do **RudBee** em ambiente local.

---

## Requisitos

Antes de começar, certifique-se de que você tem os seguintes itens instalados:

- [Node.js](https://nodejs.org/) (v16+)
- [Yarn](https://yarnpkg.com/) (ou use `npm` como alternativa)
- [Next.js CLI](https://nextjs.org/)
- [Genß](https://josearthursoares.github.io/genb-documentacao/), onde a instalação e configuração do ambiente seguirão a documentação própria.
- Um terminal com acesso ao seu projeto

---

## Instalação Local - RudBee

Siga os passos abaixo para configurar o ambiente:

### 1. Clone o repositório do RudBee (se ainda não fez)

```bash
git clone https://gitlab.brlight.com.br/lightbase1/rudbee/rudbee
```

### 2. Copie o arquivo de variáveis de ambiente

```bash
cp server/.env.dist server/.env
```

### 3. Instale as dependências presentes do projeto

```bash
yarn install
```

**Obs**: Como mencionado nas dependências, caso deseje ou prefira, pode ser utilizado o npm para instalação das dependências. Nesse caso, o comando ficaria:

```bash
npm install
```

### 4. Compile o projeto

```bash
npm run build
```

### 5. Instale o Next.js CLI globalmente (caso não tenha)

```bash
npm install -g next
```

### 6. Por fim, rode o servidor

```bash
npm run dev
```

### Dicas Úteis

Caso tenha modificado o arquivo `.env`, reinicie o servidor:

```bash
npm run build
npm start
```
