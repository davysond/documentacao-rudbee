# RudBee — Guia de Configuração do GenB

Este guia mostra como configurar e iniciar o projeto do **GenB** em ambiente local.

---

Após realizar a etapa de instalação das dependências necessárias, tópico anterior, devemos configurá-las com algumas libs adicionais. Para isso, utilizamos o [Docker](https://docs.docker.com/engine/install/) combinado, essencialmente, com o [Docker Desktop](https://docs.docker.com/engine/install/) que auxilia na visualização, configuração e gerenciamento de contêineres.

---

## Instalação Local

Siga os passos abaixo para configurar o ambiente:

### 1. Clone o repositório do GenB (se ainda não fez)

```bash
git clone https://gitlab.brlight.com.br/lightbase1/lightbase-toolkit/genb
```

### 2. Através de um Ambiente de Desenvolvimento (IDE), abra o projeto do GenB, ao qual foi clonado acima. Caso tenha o Visual Studio Code, através do Prompt de Comando do seu Sistema Operacinal

```bash
cd genb
```

```bash
code .
```

Dessa forma, o projeto será automaticamente aberto na IDE. Como mencionado, dependendo do Ambiente de Desenvolvimento que estiver utilizando os comandos podem alternar, até mesmo sendo necessário abrir o projeto de forma manual.

### 3. Navegue até a pasta gen-b (atente-se a presença do hífen)

Ao localizá-la, encontrará o arquivo `docker-compose.yml`, o mesmo é necessário para a construção dinâmica do container onde serão executadas as dependências necessárias para uso do GenB. 

### 4. Modifique o arquivo, a versão abaixo pode ser copiada e colada, substituindo o conteúdo que veio por default

```bash
version: '3.8'

networks:
  internal-host:
    driver: bridge

services:
  rabbitmq:
    image: rabbitmq:3-management
    container_name: rabbitmq
    environment:
      RABBITMQ_DEFAULT_USER: user
      RABBITMQ_DEFAULT_PASS: password
    ports:
      - "5672:5672"
      - "15672:15672"

  postgres:
    image: postgres:15.3
    container_name: postgresql2
    networks:
      - internal-host
    environment:
      POSTGRES_DB: genb
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
    ports:
      - '5433:5432'
    restart: unless-stopped

  minio:
    image: minio/minio
    container_name: minio
    environment:
      MINIO_ROOT_USER: minio
      MINIO_ROOT_PASSWORD: minio123
    ports:
      - "9000:9000"
      - "9001:9001"
    command: server /data --console-address ":9001"
    volumes:
      - miniodata:/data

  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:9.1.3
    container_name: elasticsearch2
    environment:
      - discovery.type=single-node
      - ES_JAVA_OPTS=-Xms512m -Xmx512m
      - ELASTIC_PASSWORD=changeme
      - xpack.security.enabled=true
    ports:
      - "9200:9200"
      - "9300:9300"
    volumes:
      - esdata:/usr/share/elasticsearch/data

volumes:
  pgdata:
  miniodata:
  esdata:
```

### 5. Após a substituição do conteúdo no arquivo, podemos executá-lo. Dessa forma, o container será construído automaticamente

```bash
cd gen-b
```

```bash
docker compose up -d
```

**Obs**: Caso haja algum container ou serviço rodando em uma das portas indicadas no arquivo `docker-compose.yml` será necessário pausá-lo ou, no melhor caso, modificar as portas indicadas. Além disso, o ideal é manter o Docker Desktop, caso use-o, aberto no sistema.

### 6. Ainda dentro do diretório `gen-b`, será necessário criar um arquivo com o nome `.env`, o mesmo define outras informações úteis na utilização do MINIO

Copie o cole o conteúdo abaixo:

```bash
API_GENB_PREFIX=api/v1
SERVER_PORT=3000
REQUEST_LOG=true
JWT_SECRET=#LH5hC#&qf@hSm7%x2H

POSTGRES_HOST_TEST=localhost
POSTGRES_PORT_TEST=5433
POSTGRES_USER_TEST=postgres
POSTGRES_PASSWORD_TEST=12345
POSTGRES_DATABASE_TEST=genb_test

POSTGRES_HOST=localhost
POSTGRES_PORT=5432
POSTGRES_USER=postgres
POSTGRES_PASSWORD=postgres
POSTGRES_DATABASE=genb
PORT=3000
MODE=DEV
RUN_MIGRATIONS=true

PATH=.
ELASTICSEARCH_NODE=http://localhost:9200
ELASTICSEARCH_USERNAME=elastic
ELASTICSEARCH_PASSWORD=changeme

RMQ_PRODUCER_QUEUE_FILE_EXT=queue_file_extractor
RMQ_PRODUCER_QUEUE_INDEX=queue_reindex
RMQ_PRODUCER_QUEUE_TRANSCRIBE=nonprod_genb_transcriber_queue
RMQ_PRODUCER_URL=amqp://localhost:5672
RMQ_PRODUCER_QUEUE_DURABLE=true

USER_LOGIN=root
USER_PASSWORD=12345678
USER_NAME=test

TRANSCRIBE_MULTMEDIA=false

PERSIST_FILES_USING_MINIO=false

MINIO_ACCESS_KEY=minio
MINIO_SECRET_KEY=minio123
MINIO_ENDPOINT=localhost
MINIO_PORT=9000
MINIO_SSL_ENABLED=false
MINIO_EXP_PRESIGNED_URL_IN_SECONDS=86400
```

### 7. Após a configuração dos passos acima, mantendo do Docker Destkop ainda aberto, execute

```bash
npx nx serve
```

Verifique a saídas dos logs, caso não seja exibido nenhuma mensagem apontando erro na execução, o GenB está oficialmente instalado em sua máquina.