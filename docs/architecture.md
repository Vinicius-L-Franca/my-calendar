# Architecture — My Calendar

## 1. Visão Geral

My Calendar é uma aplicação web construída com HTML5, CSS3 e JavaScript vanilla, sem dependência de frameworks frontend. O backend é simulado via JSON Server, o localStorage é utilizado como cache local dos dados e a API pública ViaCEP é consumida para completar endereços a partir do CEP.

```
┌──────────────────────────────────────────────┐
│                  Navegador                    │
│                                              │
│  ┌─────────┐  ┌──────────┐  ┌────────────┐  │
│  │  HTML   │  │   CSS    │  │     JS     │  │
│  │ (3 págs)│◄─┤ (Tokens) │◄─┤ (Módulos)  │  │
│  └─────────┘  └──────────┘  └─────┬──────┘  │
│                                   │          │
│                         fetch/async│await     │
│                                   │          │
│                    ┌──────────────┤          │
│                    │              │          │
│             ┌──────▼─────┐ ┌─────▼────────┐ │
│             │ localStorage│ │ JSON Server  │ │
│             │  (cache)    │ │  (API fake)  │ │
│             └────────────┘ └─────┬────────┘ │
│                                  └────┬─────┘
│                                 ┌─────▼──────┐
│                                 │   db.json   │
│                                 │ (banco fake)│
│                                 └────────────┘
│
│          ┌──────────────────────────────┐
│          │  ViaCEP (API pública)        │
│          │  GET /ws/{cep}/json/          │
│          └──────────────────────────────┘
└──────────────────────────────────────────────┘
```

---

## 2. Stack Tecnológica

| Camada | Tecnologia | Justificativa |
|--------|------------|---------------|
| **Estrutura** | HTML5 | 3 páginas distintas: listagem, formulário e estatísticas |
| **Estilização** | CSS3 (vanilla) | Design Tokens via CSS Variables, layout com Flexbox/Grid |
| **Lógica** | JavaScript ES6+ (vanilla) | Módulos, classes, fetch API, destructuring |
| **Backend Fake** | JSON Server | Simulação de API REST com CRUD completo |
| **Persistência** | localStorage | Cache offline e dados de preferências do usuário |
| **API Pública** | ViaCEP | Consulta assíncrona de endereço a partir do CEP |
| **Comunicação** | fetch / async/await | Requisições assíncronas à API REST e ao ViaCEP |

---

## 3. Design Tokens (CSS Variables)

Os Design Tokens definem a linguagem visual padronizada da aplicação. Todos os valores visuais são centralizados em variáveis CSS no arquivo `css/variables.css`.

### 3.1 Cores

```css
:root {
  /* Cores base */
  --color-primary: #4285F4;
  --color-bg: #FFFFFF;
  --color-bg-secondary: #F8F9FA;
  --color-text: #202124;
  --color-text-secondary: #5F6368;
  --color-border: #DADCE0;

  /* Estado */
  --color-hover: #F1F3F4;
  --color-danger: #D93025;

  /* Categorias */
  --color-work: #4285F4;        /* Azul - Trabalho */
  --color-personal: #0B8043;    /* Verde - Pessoal */
  --color-health: #D50000;      /* Vermelho - Saúde */
  --color-studies: #8E24AA;     /* Roxo - Estudos */
  --color-other: #616161;       /* Cinza - Outro */
}
```

### 3.2 Tipografia

```css
:root {
  --font-family: 'Google Sans', Roboto, 'Segoe UI', sans-serif;
  --font-size-xs: 11px;
  --font-size-sm: 12px;
  --font-size-md: 14px;
  --font-size-lg: 16px;
  --font-size-xl: 22px;
  --font-size-xxl: 28px;
  --font-weight-normal: 400;
  --font-weight-medium: 500;
  --font-weight-bold: 700;
}
```

### 3.3 Espaçamentos

```css
:root {
  --spacing-xs: 2px;
  --spacing-sm: 4px;
  --spacing-md: 8px;
  --spacing-lg: 16px;
  --spacing-xl: 24px;
  --spacing-xxl: 32px;
}
```

### 3.4 Bordas e Sombras

```css
:root {
  --border-radius: 8px;
  --border-radius-sm: 4px;
  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.1);
  --shadow-md: 0 2px 6px rgba(0, 0, 0, 0.15);
  --shadow-lg: 0 4px 12px rgba(0, 0, 0, 0.2);
}
```

---

## 4. Modelo de Dados (Diagrama ER — Mermaid)

```mermaid
erDiagram
    EVENTO {
        int id PK
        string titulo
        string data
        string horaInicio
        string horaFim
        string descricao
        string categoria
        string recorrencia
        string dataFimRecorrencia
        boolean concluido
        string cep
        string logradouro
        string cidade
        string uf
        int idEventoPai FK
    }

    EVENTO ||--o{ EVENTO : "gera instâncias recorrentes"
```

### Descrição do Relacionamento

Um evento pode gerar múltiplas instâncias futuras quando configurado como recorrente. Cada instância gerada referencia o evento original através do campo `idEventoPai`. A exclusão em cascata (excluir todas as ocorrências futuras) remove todas as instâncias cujo `idEventoPai` corresponde ao evento pai.

---

## 5. Entidades e Contratos

### 5.1 Entidade: Evento

| Campo | Tipo | Obrigatório | Validação | Descrição |
|-------|------|-------------|-----------|-----------|
| `id` | number | auto | Auto-incremento (JSON Server) | Identificador único do evento |
| `titulo` | string | sim | `/^.{3,100}$/` | Nome do evento |
| `data` | string (ISO) | sim | Formato `AAAA-MM-DD` | Data do evento |
| `horaInicio` | string | sim | Regex: `/^([01]\|2[0-3]):[0-5]\d$/` | Hora de início (24h) |
| `horaFim` | string | sim | Regex + deve ser > `horaInicio` | Hora de término (24h) |
| `descricao` | string | não | Máximo 500 caracteres | Descrição opcional do evento |
| `categoria` | string | sim | Enum válido | Categoria do evento |
| `recorrencia` | string | sim | Enum válido | Tipo de recorrência |
| `dataFimRecorrencia` | string (ISO) | condicional | Formato `AAAA-MM-DD`, > `data` | Data fim da recorrência |
| `concluido` | boolean | sim | `true` \| `false` | Indica se o evento foi realizado |
| `cep` | string | não | Regex: `/^[0-9]{5}-[0-9]{3}$/` | CEP do local do evento |
| `logradouro` | string | não | Máximo 200 caracteres | Endereço do local (preenchido via ViaCEP) |
| `cidade` | string | não | Máximo 100 caracteres | Cidade do evento |
| `uf` | string | não | Regex: `/[A-Z]{2}/` | Unidade federativa |
| `idEventoPai` | number \| null | não | FK → `EVENTO.id` ou `null` | ID do evento pai (instâncias recorrentes) |

### Enums

**Categorias:**

| Valor | Cor | CSS Variable |
|-------|-----|--------------|
| `trabalho` | Azul | `--color-work` |
| `pessoal` | Verde | `--color-personal` |
| `saude` | Vermelho | `--color-health` |
| `estudos` | Roxo | `--color-studies` |
| `outro` | Cinza | `--color-other` |

**Recorrência:**

| Valor | Descrição |
|-------|-----------|
| `none` | Sem recorrência |
| `daily` | Diário |
| `weekly` | Semanal |
| `monthly` | Mensal |
| `yearly` | Anual |

### 5.2 Exemplo de Registro

```json
{
  "id": 1,
  "titulo": "Reunião de projeto",
  "data": "2026-08-25",
  "horaInicio": "14:00",
  "horaFim": "15:30",
  "descricao": "Discussão sobre roadmap do semestre",
  "categoria": "trabalho",
  "recorrencia": "none",
  "dataFimRecorrencia": null,
  "concluido": false,
  "cep": "80010-010",
  "logradouro": "Rua das Flores",
  "cidade": "Curitiba",
  "uf": "PR",
  "idEventoPai": null
}
```

```json
{
  "id": 2,
  "titulo": "Aula de inglês",
  "data": "2026-08-25",
  "horaInicio": "18:00",
  "horaFim": "19:00",
  "descricao": "",
  "categoria": "estudos",
  "recorrencia": "weekly",
  "dataFimRecorrencia": "2026-12-31",
  "concluido": false,
  "cep": null,
  "logradouro": null,
  "cidade": null,
  "uf": null,
  "idEventoPai": null
}
```

---

## 6. Contrato da API

### 6.1 JSON Server (API Fake)

**Endpoints:**

| Método | Endpoint | Descrição | Query Params |
|--------|----------|-----------|--------------|
| `GET` | `/events` | Listar todos os eventos | `?categoria=X`, `?_sort=data` |
| `GET` | `/events/:id` | Buscar evento por ID | — |
| `POST` | `/events` | Criar novo evento | — |
| `PUT` | `/events/:id` | Atualizar evento existente | — |
| `DELETE` | `/events/:id` | Excluir evento | — |

**Estrutura de Requisição — Criar evento:**
```http
POST /events HTTP/1.1
Content-Type: application/json

{
  "titulo": "Consulta médica",
  "data": "2026-09-10",
  "horaInicio": "09:00",
  "horaFim": "10:00",
  "descricao": "Check-up anual",
  "categoria": "saude",
  "recorrencia": "yearly",
  "dataFimRecorrencia": "2030-09-10",
  "concluido": false,
  "cep": "80020-000",
  "logradouro": "Rua XV de Novembro",
  "cidade": "Curitiba",
  "uf": "PR",
  "idEventoPai": null
}
```

**Resposta (201 Created):**
```json
{
  "id": 4,
  "titulo": "Consulta médica",
  "data": "2026-09-10",
  "horaInicio": "09:00",
  "horaFim": "10:00",
  "descricao": "Check-up anual",
  "categoria": "saude",
  "recorrencia": "yearly",
  "dataFimRecorrencia": "2030-09-10",
  "concluido": false,
  "cep": "80020-000",
  "logradouro": "Rua XV de Novembro",
  "cidade": "Curitiba",
  "uf": "PR",
  "idEventoPai": null
}
```

### 6.2 ViaCEP (API Pública)

**Endpoint de consulta:**

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `GET` | `https://viacep.com.br/ws/{cep}/json/` | Consultar endereço a partir do CEP (formato `00000-000`) |

**Resposta (200 OK):**
```json
{
  "cep": "80020-000",
  "logradouro": "Rua XV de Novembro",
  "bairro": "Centro",
  "localidade": "Curitiba",
  "uf": "PR",
  "erro": false
}
```

**Tratamento de erro (CEP inexistente):** a API retorna o campo `"erro": true`. Para CEPs com formato inválido, a aplicação bloqueia a requisição na validação de formulário.

---

## 7. Estrutura de Pastas

```
my-calendar/
├── index.html                # Página principal: listagem de eventos em cards
├── evento.html               # Formulário de criação/edição de eventos
├── estatisticas.html         # Página de estatísticas da agenda
├── css/
│   ├── reset.css             # Reset do CSS (normalize)
│   ├── variables.css         # Design Tokens (CSS Variables)
│   ├── layout.css            # Header, navegação e grade responsiva
│   ├── cards.css             # Estilos dos cards de eventos
│   ├── form.css              # Estilos de formulários e estados de validação
│   └── responsive.css        # Media queries (mobile-first)
├── js/
│   ├── main.js               # Inicialização, busca e filtro por categoria
│   ├── events.js             # CRUD de eventos (fetch + localStorage)
│   ├── form.js               # Validação de formulários + consumo do ViaCEP
│   ├── recurrence.js         # Lógica de geração de eventos recorrentes
│   ├── stats.js              # Cálculo e exibição de estatísticas
│   └── storage.js            # Abstração de leitura/escrita no localStorage
├── db/
│   └── db.json               # Banco de dados do JSON Server
└── docs/
    ├── prd.md                # Product Requirements Document
    └── architecture.md       # Este documento
```

---

## 8. Padrões de Manipulação do DOM

Todos os elementos visuais da listagem são criados dinamicamente via JavaScript, sem uso de `innerHTML` para templates complexos.

### 8.1 Criação de Cards

```javascript
function createEventCard(event) {
  const cardEl = document.createElement('article');
  cardEl.classList.add('event-card', `category-${event.categoria}`);
  if (event.concluido) cardEl.classList.add('event-card--done');
  cardEl.dataset.eventId = event.id;

  const titleEl = document.createElement('h3');
  titleEl.classList.add('event-card__title');
  titleEl.textContent = event.titulo;
  cardEl.appendChild(titleEl);

  const metaEl = document.createElement('p');
  metaEl.classList.add('event-card__meta');
  metaEl.textContent = `${formatDate(event.data)} · ${event.horaInicio} – ${event.horaFim}`;
  cardEl.appendChild(metaEl);

  if (event.descricao) {
    const descEl = document.createElement('p');
    descEl.classList.add('event-card__description');
    descEl.textContent = event.descricao;
    cardEl.appendChild(descEl);
  }

  const badgeEl = document.createElement('span');
  badgeEl.classList.add('event-card__badge');
  badgeEl.textContent = CATEGORIAS[event.categoria].label;
  cardEl.appendChild(badgeEl);

  return cardEl;
}
```

### 8.2 Convenções

| Padrão | Regra |
|--------|-------|
| **Criação** | Usar `document.createElement()` para todos os elementos dinâmicos |
| **Atributos de dados** | Usar `dataset` para armazenar IDs (`data-id`) |
| **Classes** | Usar `classList.add()` / `classList.toggle()` para alternar estados |
| **Eventos** | Usar `addEventListener()` delegado no container pai quando possível |
| **Templates simples** | Permitir `innerHTML` apenas para fragments HTML curtos e controlados |

---

## 9. Uso do localStorage

### 9.1 Chaves

| Chave | Tipo | Conteúdo |
|-------|------|----------|
| `myCalendar_events` | JSON string | Array de eventos (cache do JSON Server) |
| `myCalendar_search` | string | Termo de busca persistido entre sessões |
| `myCalendar_filter` | string | Categoria selecionada no filtro (`todas` ou valor de categoria) |

### 9.2 Módulo de Abstração (`storage.js`)

```javascript
const Storage = {
  getEvents() {
    const data = localStorage.getItem('myCalendar_events');
    return data ? JSON.parse(data) : [];
  },

  saveEvents(events) {
    localStorage.setItem('myCalendar_events', JSON.stringify(events));
  },

  getSearch() {
    return localStorage.getItem('myCalendar_search') || '';
  },

  saveSearch(term) {
    localStorage.setItem('myCalendar_search', term);
  },

  getFilter() {
    return localStorage.getItem('myCalendar_filter') || 'todas';
  },

  saveFilter(category) {
    localStorage.setItem('myCalendar_filter', category);
  }
};
```

### 9.3 Fluxo de Sincronização

```
Ao carregar a listagem:
  1. Buscar eventos do JSON Server (GET /events)
  2. Salvar resultado no localStorage (cache)
  3. Renderizar cards aplicando busca e filtro salvos

Ao criar/editar/excluir/alternar conclusão de evento:
  1. Enviar requisição ao JSON Server (POST/PUT/DELETE)
  2. Receber resposta e atualizar localStorage
  3. Re-renderizar listagem ou redirecionar para a página principal

No formulário (campo CEP):
  1. Validar CEP com regex
  2. Consultar a API ViaCEP via fetch
  3. Preencher logradouro, cidade e UF automaticamente

Se JSON Server indisponível:
  1. Ler dados do localStorage (modo offline)
  2. Renderizar com dados em cache
```

---

## 10. Páginas da Aplicação

| Página | Descrição | Scripts |
|--------|-----------|---------|
| `index.html` | Listagem de eventos em cards com busca, filtro por categoria e controle de conclusão | `main.js`, `events.js`, `storage.js` |
| `evento.html` | Formulário de criação/edição com validação (HTML nativo + regex) e consulta ViaCEP | `form.js`, `events.js`, `recurrence.js`, `storage.js` |
| `estatisticas.html` | Resumo da agenda: total de eventos, por categoria, concluídos e pendentes | `stats.js`, `events.js`, `storage.js` |