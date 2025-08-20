# 📘 Documentação de Swagger — Fluxo e Detalhamento

Esta documentação explica, passo a passo, como adicionar/alterar documentação OpenAPI/Swagger no projeto. Cada etapa tem exemplo e uma explicação detalhada de **todos** os elementos aninhados usados nos exemplos.

---

## 📑 Sumário
- [📂 Estrutura de Pastas](#📂-estrutura-de-pastas)  
- [1️⃣ - Criar um Schema](#1️⃣---criar-um-schema)  
- [2️⃣ - Criar um Arquivo de Rotas (paths)](#2️⃣---criar-um-arquivo-de-rotas-paths)  
- [3️⃣ - Preencher a Documentação do Endpoint](#3️⃣---preencher-a-documentação-do-endpoint)  
- [4️⃣ - Modificar `swagger.yaml` para incorporar mudanças](#4️⃣---modificar-swaggeryaml-para-incorporar-mudanças)  
- [🔎 Deep dive — Campos de Schema (tudo aninhado)](#🔎-deep-dive-—-campos-de-schema-tudo-aninhado)  
- [🛣️ Deep dive — Paths / Operation / Parameters / RequestBody / Responses](#🛣️-deep-dive-—-paths--operation--parameters--requestbody--responses)  
- [⚙️ Comandos e Observações Finais](#⚙️-comandos-e-observações-finais)

---

## 📂 Estrutura de Pastas

```
docs/
├── swagger.yaml                    # Arquivo principal (referência)
├── components/
│   ├── schemas/                    # Definição dos modelos (schemas)
│   └── responses/                  # Respostas reutilizáveis
└── paths/                          # Endpoints da API (agrupados por funcionalidade)
```

> ⚠️ **IMPORTANTE:** `swagger-bundle.yaml` NÃO deve ser editado manualmente.  
> Faça alterações nos arquivos dentro de `docs/components/` e `docs/paths/` e depois rode o bundler (`npm run bundle-swagger`).

---

## 1️⃣ - Criar um Schema

> **Objetivo:** declarar a estrutura do objeto (model). O schema **não** contém parâmetros de rota — apenas a definição do objeto (propriedades, tipos, exemplos, validações).

**Local:** `docs/components/schemas/`  
**Exemplo de arquivo:** `docs/components/schemas/Turma.yaml`

```yaml
# docs/components/schemas/Turma.yaml
Turma:
  type: object
  required:
    - id
    - nome
  properties:
    id:
      type: integer
      description: Identificador único da turma
      example: 123
    nome:
      type: string
      description: Nome da turma
      example: "Turma A"
    descricao:
      type: string
      description: Descrição opcional da turma
      example: "Turma do primeiro semestre"
    vigente:
      type: boolean
      description: Indica se a turma está ativa
      example: true
    dataInicio:
      type: string
      format: date
      description: Data de início da turma (YYYY-MM-DD)
      example: "2025-02-01"
```

### ✅ O que foi usado no exemplo (campo a campo)
- `type: object` — define que o recurso é um objeto JSON.  
- `required` — lista de propriedades obrigatórias (`id`, `nome`).  
- `properties` — mapa de propriedades:
  - `id` — `type: integer`, `description`, `example`. Identificador único.
  - `nome` — `type: string`, `description`, `example`.
  - `descricao` — `type: string`, opcional.
  - `vigente` — `type: boolean` (true/false).
  - `dataInicio` — `type: string` + `format: date` (indica formato YYYY-MM-DD).
- `example` — ajuda a UI a mostrar valores exemplares.

---

## 2️⃣ - Criar um Arquivo de Rotas (paths)

> **Objetivo:** agrupar endpoints por recurso. O arquivo `paths` **referencia** schemas via `$ref` — não redefine o schema.

**Local:** `docs/paths/`  
**Exemplo de arquivo:** `docs/paths/turmas.yaml`

```yaml
# docs/paths/turmas.yaml
/turmas:
  get:
    summary: Lista todas as turmas
    tags: [Turmas]
    responses:
      '200':
        description: Lista de turmas retornada com sucesso
        content:
          application/json:
            schema:
              type: array
              items:
                $ref: '../components/schemas/Turma.yaml#/Turma'

  post:
    summary: Cria uma nova turma
    tags: [Turmas]
    requestBody:
      required: true
      content:
        application/json:
          schema:
            $ref: '../components/schemas/Turma.yaml#/Turma'
    responses:
      '201':
        description: Turma criada com sucesso
```

### ✅ O que foi usado no exemplo (campo a campo)
- `paths` — chave URL (`/turmas`).  
- `get`, `post` — operações (HTTP methods).  
- `summary` — descrição curta da operação.  
- `tags` — agrupa operações na UI.  
- `responses` — define códigos de resposta e `content`.  
- Em `get` o `schema` é `type: array` com `items` referenciando `Turma` via `$ref`.  
- Em `post` `requestBody` contém `content` e `schema` com `$ref` para o schema `Turma`.

---

## 3️⃣ - Preencher a Documentação do Endpoint

> **Objetivo:** detalhar cada rota seguindo o checklist combinado — para cada rota, inclua **obrigatoriamente**:

**Parâmetros esperados para preenchimento:**
- Método HTTP (GET, POST, PUT, DELETE, etc.)
- Caminho da rota (ex: `/turmas`, `/turmas/{id}`)
- Parâmetros: `query`, `path`, `header` (quando aplicável)
- `requestBody` (quando aplicável) / schema de entrada via `$ref`
- Schema de saída (response) via `$ref`
- Códigos de status HTTP retornados (200, 201, 400, 404, 500, etc.)
- Mensagens de erro possíveis (descrições e/ou referências para `components/responses`)
- Requisitos especiais (ex: autenticação `security`)

**Exemplo detalhado:** trecho de `docs/paths/turmas.yaml`

```yaml
# docs/paths/turmas.yaml (trecho detalhado)
/turmas/{id}:
  get:
    tags: [Turmas]
    summary: Recupera uma turma pelo ID
    description: Retorna a turma com opção de incluir lista de alunos e metadados.
    operationId: getTurmaById
    parameters:
      - name: id
        in: path
        description: ID da turma
        required: true
        schema:
          type: integer
      - name: incluirAlunos
        in: query
        description: Se true, inclui lista de alunos
        required: false
        schema:
          type: boolean
    responses:
      '200':
        description: Turma retornada com sucesso
        headers:
          X-Request-ID:
            description: ID da requisição para troubleshooting
            schema:
              type: string
        content:
          application/json:
            schema:
              $ref: '../components/schemas/Turma.yaml#/Turma'
            examples:
              default:
                value:
                  id: 10
                  nome: "Turma A"
                  alunos: [{ id: 1, nome: "João" }]
      '401':
        $ref: '../components/responses/Unauthorized.yaml#/Unauthorized'
    security:
      - bearerAuth: []
    servers:
      - url: https://api.example.com/v1
  put:
    summary: Atualiza uma turma pelo ID
    parameters:
      - name: id
        in: path
        required: true
        schema:
          type: integer
        description: ID da turma
    requestBody:
      required: true
      content:
        application/json:
          schema:
            $ref: '../components/schemas/Turma.yaml#/Turma'
    responses:
      '200':
        description: Turma atualizada com sucesso
      '400':
        description: Requisição inválida (dados faltando ou formato incorreto)
```

### ✅ O que foi usado no exemplo (campo a campo)
- `operationId` — identificador único (útil para gerar clients).  
- `parameters` — objetos com campos: `name`, `in`, `description`, `required`, `schema`.  
- `schema` dentro de `parameters` — descreve o tipo do parâmetro (ex: `integer`, `boolean`).  
- `requestBody` — contém `content` com `media type` e `schema` (pode ser `$ref`).  
- `responses` — cada código requer `description`; pode conter `headers`, `content`, `examples` ou `$ref` para `components/responses`.  
- `security` — lista de requisitos de segurança (ex: `bearerAuth`).  
- `servers` — sobrescreve servidores apenas para essa operação.

---

## 4️⃣ - Modificar `swagger.yaml` para incorporar as mudanças

> **Objetivo:** o arquivo raiz reúne metadados e pontos de entrada. Você precisa inserir cada path manualmente nele se usar o bundler — o bundler resolve e os concatena no arquivo de saída.

**Exemplo simplificado de `docs/swagger.yaml`:**

```yaml
openapi: 3.0.0
info:
  title: API Escola
  version: "1.0.0"
  description: API de exemplo para gerenciar turmas e alunos.
servers:
  - url: http://localhost:3333
paths: 
  /turmas:
    $ref: './paths/turmas.yaml#/~1turmas' # ~1 == /
  /turmas/{id}:
    $ref: './paths/turmas.yaml#/~1turmas~1{id}'
components:
  schemas:
    Turma:
      $ref: './components/schemas/Turma.yaml'
  responses:
    $ref: './components/responses/errors.yaml'
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT
security:
  - bearerAuth: []
tags:
  - name: Turmas
    description: Operações relacionadas a turmas
```

### ✅ O que foi usado no exemplo (campo a campo)
- `openapi`, `info` — metadados gerais (título, versão, descrição).  
- `servers` — lista de URLs de execução.  
- `components.securitySchemes` — define `bearerAuth` (JWT) e outros.  
- `paths`, `components/schemas`, `components/responses` — normalmente são preenchidos/mesclados pelo bundler a partir dos arquivos em `docs/paths/` e `docs/components/`.

- **Observação:** Em paths->/turmas->$ref para acessar um campo chamado /turmas dentro do arquivo, escreva: 
```
#/paths/~1turmas
```

ficando:

```
  /turmas:
    $ref: './paths/turmas.yaml#/~1turmas' # ~1 == /

```

Se quiser saber mais, consulte o [RFC](https://pt.wikipedia.org/wiki/Request_for_Comments).

 E o padrão [RFC 6901](https://datatracker.ietf.org/doc/html/rfc6901#section-3).


---

## 🔎 Deep dive — Campos de Schema (tudo aninhado)

Abaixo explico **cada** campo que pode aparecer dentro de um schema (nível de propriedade incluso), com exemplos.

### 🧩 Campos de alto nível no schema de um `object`
- `type` — tipo do schema (`object`, `string`, `integer`, `array`, `boolean`, `number`).  
- `required` — array com nomes de propriedades obrigatórias.  
- `properties` — mapa `nomeDaPropriedade: schema`.

### 🔹 Campos possíveis dentro de cada propriedade (detalhado)
- `type` — tipo da propriedade.
- `format` — formato indicado (ex: `date-time`, `date`, `email`, `uuid`, `int64`).
- `description` — explicação do campo.
- `example` — exemplo de valor.
- `enum` — valores permitidos (lista).
- `nullable` — `true` se aceita `null`.
- `readOnly` — `true` se só aparece nas respostas.
- `writeOnly` — `true` se só faz sentido no request (ex.: senha).
- `default` — valor padrão.
- `minimum`, `maximum` — limites numéricos.
- `minLength`, `maxLength` — limites de tamanho para strings.
- `pattern` — regex para validar string.
- `items` — quando `type: array`, descreve cada item (pode ser `type` ou `$ref`).
- `additionalProperties` — quando `type: object`, controla propriedades extras (boolean ou schema).
- `allOf`, `oneOf`, `anyOf`, `not` — composição de schemas (herança, variantes, negação).
- `$ref` — referência a schema externo (usar caminhos relativos para o bundler).
- `discriminator` — usado com polymorphism (indica propriedade que define o tipo variante).

**Exemplos aninhados:**

```yaml
# Array de objetos inline
alunos:
  type: array
  items:
    type: object
    required: [id, nome]
    properties:
      id:
        type: integer
        example: 10
      nome:
        type: string
        example: "João"

# Usando $ref para reaproveitar schema
alunosRef:
  type: array
  items:
    $ref: '../components/schemas/Aluno.yaml#/Aluno'
```

---

## 🛣️ Deep dive — Paths / Operation / Parameters / RequestBody / Responses

### 🧭 Operation object (campos que podem aparecer dentro de um método: `get`, `post`, etc.)
- `tags`, `summary`, `description`, `operationId`  
- `parameters` (lista de Parameter objects)  
- `requestBody` (quando há payload)  
- `responses` (Response Object com códigos)  
- `security` (autenticação)  
- `servers` (override por operação)  
- `callbacks` (webhooks assíncronos)  
- `deprecated` (boolean)

### 🔎 Parameter object (detalhes)
- `name` — nome do parâmetro.  
- `in` — localização (`path`, `query`, `header`, `cookie`).  
- `description` — explicação.  
- `required` — obrigatório. (path parameters: sempre `true`)  
- `deprecated` — se obsoleto.  
- `allowEmptyValue` — permite valor vazio (somente query).  
- `style` — como serializar (form, simple, matrix, label, deepObject, etc.).  
- `explode` — controla serialização para arrays/objetos (`true`/`false`).  
- `allowReserved` — permite caracteres reservados em query sem encoding.  
- `schema` — define o tipo/format do parâmetro.  
- `example` / `examples` — exemplos.

**Exemplo:**

```yaml
parameters:
  - name: id
    in: path
    required: true
    schema:
      type: integer
    description: ID do recurso
  - name: filter
    in: query
    required: false
    schema:
      type: string
    style: form
    explode: true
    description: Filtro de busca
```

### 📨 RequestBody object (detalhes)
- `description` — explicação do payload.  
- `content` — mapa `mediaType -> MediaTypeObject` (ex.: `application/json`).  
- `required` — se o body é obrigatório.

**MediaTypeObject** contém:
- `schema` — Schema object ou `$ref`.  
- `examples` / `example` — exemplos de payload.  
- `encoding` — usado para `multipart/form-data` (controla contentType, headers, style, explode).

**Exemplo:**

```yaml
requestBody:
  required: true
  content:
    application/json:
      schema:
        $ref: '../components/schemas/Turma.yaml#/Turma'
      examples:
        exemploSimples:
          value:
            id: 1
            nome: "Turma A"
```

### 📬 Responses (detalhes)
Cada código aponta para um **Response Object** com:
- `description` — obrigatório.  
- `headers` — mapa de headers customizados (cada header com seu `schema`).  
- `content` — media types com `schema` e `examples`.  
- `links` — URLs/endpoints relacionados (útil HATEOAS).

**Exemplo com headers e exemplo:**

```yaml
responses:
  '200':
    description: OK
    headers:
      X-Total-Count:
        description: Total de itens
        schema:
          type: integer
    content:
      application/json:
        schema:
          type: array
          items:
            $ref: '../components/schemas/Turma.yaml#/Turma'
        examples:
          sample:
            value: [{ id: 1, nome: "Turma A" }]
  '400':
    $ref: '../components/responses/BadRequest.yaml#/BadRequest'
```

### 🔁 Callbacks & Links
- `callbacks` — descreve endpoints externos que a API (assíncrona) pode chamar.  
- `links` — descreve possíveis links a partir de uma resposta que mapeiam para outra operação.

### ♻️ Reuso com `components/responses`
Crie respostas reutilizáveis em `docs/components/responses/` (ex.: `BadRequest.yaml`, `Unauthorized.yaml`) e referencie via `$ref` dentro de `responses`.

---

## ⚙️ Comandos e Observações Finais

```bash
# Instalação (se necessário)
npm install

# Validar sintaxe dos YAMLs
npm run validate-swagger

# Empacotar/Bundle (gera swagger-bundle.yaml)
npm run bundle-swagger
```

- Depois de bundle, abra a UI Swagger:  
  `http://localhost:3333/api-docs/`
- **NUNCA** edite `swagger-bundle.yaml` manualmente — faça alterações nos arquivos em `docs/paths/` e `docs/components/` e gere o bundle.
- Use referências relativas (ex.: `../components/schemas/Turma.yaml#/Turma`) para que o bundler resolva corretamente.
- Para endpoints que exigem autenticação, defina `components.securitySchemes` (ex.: `bearerAuth`) e adicione `security` nas operações.
