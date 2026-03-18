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
