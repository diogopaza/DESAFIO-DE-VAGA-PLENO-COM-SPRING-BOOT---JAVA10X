# ⌚ API de Controle de Estoque de Relógios

API REST desenvolvida com **Spring Boot** para gerenciamento de estoque de relógios.

Projeto focado em boas práticas de backend:

* Arquitetura em camadas
* DTO + Mapper
* Filtros dinâmicos
* Paginação e ordenação
* Tratamento global de erros

---

## 🚀 Tecnologias

* Java 17+
* Spring Boot (Web, Data JPA, Validation)
* PostgreSQL
* Lombok

---

## 🧱 Arquitetura

Organização por responsabilidade:

```
com.market.watchapi
  ├── entity/
  ├── repository/
  ├── service/
  ├── controller/
  ├── dto/
  ├── mapper/
  ├── config/
  └── exception/
```

### Responsabilidades

| Camada     | Responsabilidade                       |
| ---------- | -------------------------------------- |
| entity     | Entidades JPA e enums                  |
| repository | Acesso a dados (Spring Data JPA)       |
| service    | Regras de negócio, filtros e ordenação |
| controller | Endpoints REST                         |
| dto        | Contratos de entrada e saída           |
| mapper     | Conversão Entity ↔ DTO                 |
| exception  | Exceções e tratamento global           |
| config     | Seed de dados iniciais                 |

---

## ⚙️ Funcionalidades

### 🔍 Listagem com filtros

* Paginação (`pagina`, `porPagina`)

* Busca textual (`busca`)

* Filtros combináveis:

  * marca
  * tipoMovimento
  * materialCaixa
  * tipoVidro
  * resistência (min/max)
  * preço (min/max)
  * diâmetro (min/max)

* Ordenação:

  * `newest`
  * `price_asc`
  * `price_desc`
  * `diameter_asc`
  * `wr_desc`

---

### 🔎 Buscar por ID

Retorna um relógio específico.

---

### 🛠️ CRUD completo

* Criar
* Atualizar
* Remover

---

## 🌐 Endpoints

### Base URL

```
http://localhost:8080
```

---

### 📄 Listar relógios

```
GET /api/relogios
```

#### Query Params

| Parâmetro      | Tipo   | Padrão | Descrição                   |
| -------------- | ------ | ------ | --------------------------- |
| pagina         | int    | 1      | Página atual                |
| porPagina      | int    | 12     | Máx 60                      |
| busca          | string | -      | Busca textual               |
| marca          | string | -      | Filtro por marca            |
| tipoMovimento  | string | -      | quartz / automatic / manual |
| materialCaixa  | string | -      | steel / titanium / resin    |
| tipoVidro      | string | -      | mineral / sapphire          |
| resistenciaMin | int    | -      | Resistência mínima          |
| resistenciaMax | int    | -      | Resistência máxima          |
| precoMin       | long   | -      | Preço mínimo                |
| precoMax       | long   | -      | Preço máximo                |
| diametroMin    | int    | -      | Diâmetro mínimo             |
| diametroMax    | int    | -      | Diâmetro máximo             |
| ordenar        | string | newest | Ordenação                   |

---

### 🔎 Buscar por ID

```
GET /api/relogios/{id}
```

---

### ➕ Criar relógio

```
POST /api/relogios
```

---

### ✏️ Atualizar relógio

```
PUT /api/relogios/{id}
```

---

### ❌ Remover relógio

```
DELETE /api/relogios/{id}
```

---

## 📦 Exemplo de Response

```json
{
  "itens": [
    {
      "id": "uuid",
      "marca": "Seiko",
      "modelo": "Diver 200m",
      "tipoMovimento": "automatic",
      "materialCaixa": "steel",
      "tipoVidro": "mineral",
      "resistenciaAguaM": 200,
      "diametroMm": 42,
      "precoEmCentavos": 159990,
      "etiquetaResistenciaAgua": "mergulho",
      "pontuacaoColecionador": 73
    }
  ],
  "total": 3
}
```

---

## 🧠 Campos Calculados

### etiquetaResistenciaAgua

| Resistência | Classificação |
| ----------- | ------------- |
| < 50        | respingos     |
| 50–99       | uso_diario    |
| 100–199     | natacao       |
| ≥ 200       | mergulho      |

---

### pontuacaoColecionador

Pontuação baseada em características do relógio:

* Vidro safira → +25
* Resistência ≥ 100 → +15
* Resistência ≥ 200 → +10 bônus
* Movimento automático → +20
* Caixa:

  * aço → +10
  * titânio → +12
* Diâmetro entre 38–42mm → +8

---

## ⚠️ Tratamento de Erros

### 404 — Não encontrado

```json
{
  "status": 404,
  "mensagem": "Relógio não encontrado"
}
```

---

### 400 — Erro de validação

```json
{
  "status": 400,
  "mensagem": "Erro de validação"
}
```

---

### 400 — Enum inválido

```json
{
  "status": 400,
  "mensagem": "Tipo de movimento inválido"
}
```

---

## 🌱 Seed de Dados

Ao iniciar a aplicação:

* Se a tabela estiver vazia, dados de exemplo são inseridos automaticamente.

---

## 🧩 Decisões Técnicas

* **DTO + Mapper manual**

  * Maior controle
  * Simples para desafios

* **JPA Specifications**

  * Filtros dinâmicos e escaláveis
  * Evita `if-else` gigante

* **Service centralizado**

  * Controller mais limpo

* **Limite de paginação (60)**

  * Evita abuso da API

* **Enums com fromApi/toApi**

  * Evita strings soltas
  * Mantém consistência

---

## 📌 Objetivo

Projeto desenvolvido para prática e demonstração de habilidades em:

* Backend com Spring Boot
* Design de APIs REST
* Boas práticas de arquitetura
* Filtros dinâmicos com JPA

---

🧭 Plano de Avaliação por Etapas
📊 Sistema de notas

Cada etapa vai ter nota de 0 a 10, baseada em:

Qualidade do código (40%)

Boas práticas (30%)

Clareza/organização (20%)

Maturidade técnica (10%)

👉 E eu também vou te dizer:

nível: ❌ fraco | ⚠️ ok | ✅ bom | 🔥 forte

🚀 ETAPAS
🟢 Etapa 1 — Estrutura + Entity + Enum

Objetivo: base sólida

Você entrega:

estrutura de pacotes

entidade Relogio

enums (tipoMovimento, etc.)

Vou avaliar:

modelagem correta

uso de JPA (@Entity, @Id, etc.)

uso de enums (fromApi/toApi 🔥)

organização de pacotes

🟢 Etapa 2 — Repository + Seed inicial

Objetivo: persistência funcionando

Você entrega:

RelogioRepository

classe de seed (dados iniciais)

Vou avaliar:

uso correto do Spring Data

design do repository

seed limpo (sem gambiarra)

🟡 Etapa 3 — CRUD básico (sem DTO ainda)

Objetivo: fluxo funcionando

Você entrega:

Service

Controller

endpoints CRUD funcionando

Vou avaliar:

separação de responsabilidades

controller fino (importante!)

service com regras

🟡 Etapa 4 — DTO + Mapper

Objetivo: maturidade de API

Você entrega:

DTO de entrada/saída

mapper manual

Vou avaliar:

separação entity vs DTO

mapper limpo

código legível (sem bagunça)

🟠 Etapa 5 — Paginação + Ordenação

Objetivo: API mais profissional

Você entrega:

paginação (Pageable)

ordenação

Vou avaliar:

uso correto do Spring

não reinventar roda

resposta bem estruturada

🔴 Etapa 6 — Filtros simples

Objetivo: começar lógica dinâmica

Você entrega:

filtros básicos funcionando (marca, tipo, etc.)

Vou avaliar:

clareza

organização (sem if gigante ainda)

🔥 Etapa 7 — Specification (nível avançado)

Objetivo: diferencial real

Você entrega:

filtros combináveis com JpaSpecificationExecutor

Vou avaliar PESADO aqui:

qualidade da implementação

reaproveitamento

elegância da solução

👉 Essa etapa vale MUITO no mercado

🟣 Etapa 8 — Campos calculados (mapper/service)

Objetivo: lógica de negócio

Você entrega:

etiquetaResistenciaAgua

pontuacaoColecionador

Vou avaliar:

clareza da lógica

organização (não poluir mapper)

código limpo

🟤 Etapa 9 — Validação + Exception Handler

Objetivo: robustez

Você entrega:

validações (@Valid, etc.)

@ControllerAdvice

Vou avaliar:

qualidade das mensagens

tratamento de erro

padrão de resposta

⚫ Etapa 10 — Refinamento final

Objetivo: nível profissional

Você melhora:

nomes

organização

pequenos detalhes

Vou avaliar:

“cara de projeto de empresa”

🧠 Etapa EXTRA (IMPORTANTE PRA VIDA REAL)
💬 Etapa 11 — Comunicação assíncrona (🔥 diferencial forte)

Você NÃO vai codar, vai explicar:

Responda como se estivesse no Slack da empresa:

“Explique como sua API funciona, decisões que você tomou e o que melhoraria com mais tempo”

Vou avaliar:

clareza

objetividade

pensamento de engenheiro (não só codador)

👉 Isso aqui separa quem passa em entrevista

🏁 Resultado final

No final eu te dou:

📊 média geral

📈 seu nível (Júnior | Júnior forte | Pleno)

🎯 o que falta pra próximo nível

💬 como você se vender em entrevista
