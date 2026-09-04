# My Calendar

**Autor:** Vinicius Liepienski de França

---

## Descrição

My Calendar é uma aplicação web de agenda pessoal que permite ao usuário criar, visualizar, editar e excluir compromissos. Os eventos são organizados como cards em um layout responsivo, com busca por texto, filtro por categoria, marcação de conclusão, recorrência, preenchimento de endereço via CEP (API ViaCEP) e página de estatísticas — tudo acessível diretamente pelo navegador.

---

## Prototipação no Figma

https://stitch.withgoogle.com/projects/6869356017674285767

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
| [ViaCEP](https://viacep.com.br/) | API pública de consulta de endereço por CEP (sem necessidade de chave) | Sem instalação |

---

## Link para Produção

> URL do GitHub Pages (a definir)

---

## Checklist de Funcionalidades

- [ ] Listar eventos em cards (layout responsivo em grade)
- [ ] Criar evento (título, data, hora, descrição, categoria, recorrência)
- [ ] Editar evento existente
- [ ] Excluir evento com confirmação
- [ ] Buscar eventos por texto (título/descrição)
- [ ] Filtrar eventos por categoria
- [ ] Marcar evento como concluído
- [ ] Categorias com cores (Trabalho, Pessoal, Saúde, Estudos, Outro)
- [ ] Recorrência (diária, semanal, mensal, anual)
- [ ] Busca de endereço por CEP via API ViaCEP
- [ ] Página de estatísticas (total, por categoria, concluídos)
- [ ] Validação de formulários (HTML nativo + regex)
- [ ] Persistência via JSON Server + cache localStorage
- [ ] Layout responsivo (mobile e desktop)

---

## Indicadores de Desempenho (ID) — Resultados de Aprendizagem (RA)

> A aplicação deve ser implementada contendo as funcionalidades referentes aos tópicos apresentados a seguir, estruturados de acordo com os Resultados de Aprendizagem da Matriz por Competências.

### RA1 — Utilizar Frameworks CSS para estilização de elementos HTML e criação de layouts responsivos

- [ ] **ID 01** — Prototipa interfaces adaptáveis para no mínimo os tamanhos de tela mobile e desktop
- [ ] **ID 02** — Implementa layout responsivo com Framework CSS usando Flexbox ou Grid do próprio framework
- [ ] **ID 03** — Implementa layout responsivo com CSS puro, usando Flexbox ou Grid Layout
- [ ] **ID 04** — Utiliza componentes prontos de um Framework CSS e componentes JavaScript do framework
- [ ] **ID 05** — Cria layout fluido usando unidades relativas (vw, vh, %, em, rem) no lugar de unidades fixas (px)
- [ ] **ID 06** — Aplica um Design System consistente (cores, tipografia, padrões de componentes) em toda a aplicação
- [ ] **ID 07** — Utiliza Sass (SCSS) com ou sem framework, aplicando variáveis, mixins e funções
- [ ] **ID 08** — Aplica tipografia responsiva (media queries mobile first) ou tipografia fluida (clamp())
- [ ] **ID 09** — Aplica técnicas de responsividade de imagens usando CSS (object-fit, containers com unidades relativas)
- [ ] **ID 10** — Otimiza imagens usando formatos modernos (WebP) e carregamento adaptativo (srcset, picture)

### RA2 — Realizar tratamento de formulários e aplicar validações customizadas no lado cliente

- [ ] **ID 11** — Implementa validação HTML nativa (campos obrigatórios, tipos, limites de caracteres) com mensagens de erro/sucesso
- [ ] **ID 12** — Aplica expressões regulares (REGEX) para validações customizadas
- [ ] **ID 13** — Utiliza elementos de seleção em formulários (checkbox, radio, select) para coleta de dados
- [ ] **ID 14** — Implementa leitura e escrita no Web Storage (localStorage/sessionStorage) para persistir dados localmente

### RA3 — Aplicar ferramentas para otimização do processo de desenvolvimento web

- [ ] **ID 15** — Configura ambiente com Node.js e NPM para gerenciamento de pacotes e dependências
- [ ] **ID 16** — Utiliza boas práticas de versionamento no Git/GitHub
- [ ] **ID 17** — Mantém um README.md padronizado, conforme template da disciplina, com checklist preenchido
- [ ] **ID 18** — Organiza arquivos do projeto de forma modular, seguindo padrão de exemplo fornecido
- [ ] **ID 19** — Configura linters e formatadores (ESLint, Prettier) para manter qualidade e padronização do código

### RA4 — Aplicar bibliotecas de funções e componentes em JavaScript para aprimorar a interatividade

- [ ] **ID 20** — Utiliza jQuery para manipulação do DOM e interatividade (eventos, animações, manipulação de elementos)
- [ ] **ID 21** — Integra e configura um plugin jQuery relevante ou outra biblioteca de funções

### RA5 — Efetuar requisições assíncronas para uma API fake e APIs públicas

- [ ] **ID 22** — Realiza requisições assíncronas para uma API fake (JSON Server) para persistir dados de um formulário
- [ ] **ID 23** — Realiza requisições assíncronas para uma API fake para exibir dados na página
- [ ] **ID 24** — Realiza requisições assíncronas para APIs públicas reais, exibindo os dados e tratando erros

---

## Estrutura do Projeto

```
my-calendar/
├── index.html                # (a criar) Lista de eventos em cards
├── evento.html               # (a criar) Formulário de criação/edição
├── estatisticas.html         # (a criar) Página de estatísticas
├── css/                      # (a criar)
│   ├── reset.css
│   ├── variables.css
│   ├── layout.css
│   ├── cards.css
│   ├── form.css
│   └── responsive.css
├── js/                       # (a criar)
│   ├── main.js
│   ├── events.js
│   ├── form.js
│   ├── recurrence.js
│   ├── stats.js
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
3. A página `evento.html` consulta a API ViaCEP (https://viacep.com.br) ao informar um CEP

> **Nota:** As páginas `index.html`, `evento.html`, `estatisticas.html`, as pastas `css/`, `js/`, `db/` e os arquivos da aplicação serão criados durante a implementação. Atualmente o repositório contém apenas a documentação (`docs/`).

---

## Telas da Aplicação

> Imagens das telas serão adicionadas após a implementação.

---

## Documentação

- [PRD (Product Requirements Document)](docs/prd.md)
- [Architecture (Especificação Técnica)](docs/architecture.md)
