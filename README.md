# My Calendar

**Autor:** Vinicius Liepienski de França

---

## Descrição

My Calendar é uma aplicação web de calendário/agenda pessoal que permite ao usuário criar, visualizar, editar e excluir compromissos. A aplicação oferece três visões (mensal, semanal e diária), categorização de eventos por cores, recorrência simples e drag-and-drop para reagendamento rápido — tudo acessível diretamente pelo navegador.

---

## Prototipação no Figma

> Link para o projeto no Figma (a definir)

---

## Design System

> Documento de referência do Design System: [docs/architecture.md](docs/architecture.md#3-design-tokens-css-variables)

---

## Framework CSS

CSS Vanilla (sem framework externo). Design Tokens implementados via CSS Variables.

---

## Dependências

| Dependência | Descrição | Status |
|-------------|-----------|--------|
| [JSON Server](https://github.com/typicode/json-server) | API REST fake para simulação de backend | A instalar |

---

## Link para Produção

> URL do GitHub Pages (a definir)

---

## Checklist de Funcionalidades

- [ ] Criar evento (título, data, hora, descrição, categoria)
- [ ] Editar evento existente
- [ ] Excluir evento com confirmação
- [ ] Visão mensal com grid de dias
- [ ] Visão semanal com faixas horárias
- [ ] Visão diária com faixas horárias
- [ ] Navegação entre períodos (setas anterior/próximo)
- [ ] Botão "Hoje" para voltar à data atual
- [ ] Categorias com cores (Trabalho, Pessoal, Saúde, Estudos, Outro)
- [ ] Recorrência (diária, semanal, mensal, anual)
- [ ] Drag-and-drop nas visões semanal e diária
- [ ] Validação de formulários com regex
- [ ] Persistência via JSON Server + cache localStorage
- [ ] Layout responsivo (mobile e desktop)

---

## Estrutura do Projeto

```
my-calendar/
├── index.html                # (a criar)
├── css/                      # (a criar)
│   ├── reset.css
│   ├── variables.css
│   ├── calendar.css
│   ├── modal.css
│   ├── form.css
│   └── responsive.css
├── js/                       # (a criar)
│   ├── main.js
│   ├── calendar.js
│   ├── events.js
│   ├── modal.js
│   ├── form.js
│   ├── recurrence.js
│   ├── dragdrop.js
│   └── storage.js
├── db/                       # (a criar)
│   └── db.json
└── docs/
    ├── prd.md
    └── architecture.md
```

---

## Instruções de Execução

### Pré-requisitos

- [Node.js](https://nodejs.org/) (v18+)
- npm

### Instalação

```bash
# Clonar o repositório
git clone https://github.com/viniciusfranca/my-calendar.git
cd my-calendar

# Instalar JSON Server como dependência de desenvolvimento
npm install --save-dev json-server

# Iniciar o JSON Server na porta 3000
npx json-server --watch db/db.json --port 3000
```

### Execução

1. Em outro terminal, abra o arquivo `index.html` no navegador ou use um servidor local (ex: Live Server do VS Code)
2. A aplicação se comunica com o JSON Server em `http://localhost:3000`

> **Nota:** As pastas `css/`, `js/`, `db/` e os arquivos da aplicação serão criados durante a implementação. Atualmente o repositório contém apenas a documentação (`docs/`).

---

## Telas da Aplicação

> Imagens das telas serão adicionadas após a implementação.

---

## Documentação

- [PRD (Product Requirements Document)](docs/prd.md)
- [Architecture (Especificação Técnica)](docs/architecture.md)
