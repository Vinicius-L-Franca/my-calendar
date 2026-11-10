# 🎨 Design System - My Calendar

Neste projeto, utilizamos um framework UI e aplicamos customizações, via Sass, para refletir a identidade visual de uma agenda limpa e organizada por cores.

### 1. Framework Base

- **Framework escolhido:** Bootstrap 5.3.8
- **Motivação:** O grid mobile-first e os componentes prontos (card, navbar, dropdown, modal, formulários) cobrem as três telas da agenda. Além disso, as cores, a fonte e as bordas do Bootstrap são variáveis Sass, que eu troco pelas do Design System antes de compilar.

### 2. Paleta de Cores (Customização)

As cores seguem um visual claro, em que a cor só aparece onde ajuda a localizar uma informação:

- **Cor Primária:** `#4285F4` _(azul)_
  - _Uso:_ Nome do site, link da página atual, botão "Salvar evento" e item ativo do menu do celular.
- **Cor de Fundo da Página:** `#F8F9FA` _(cinza muito claro)_
  - _Uso:_ Fundo de todas as páginas, para destacar os cards brancos.
- **Cor de Fundo dos Componentes:** `#FFFFFF` _(branco)_
  - _Uso:_ Cards, campos do formulário, cabeçalho, rodapé e menus.
- **Cor do Texto:** `#202124` _(quase preto)_ e **Texto Secundário:** `#5F6368` _(cinza)_
  - _Uso:_ O texto principal e, em cinza, as informações de apoio (horário, local, ícones, links do rodapé).
- **Cor de Borda:** `#DADCE0` e **Cor de Hover:** `#F1F3F4`
  - _Uso:_ Bordas dos cards e campos; fundo do card "Adicionar novo evento" com o mouse em cima.
- **Cor de Perigo:** `#D93025` _(vermelho)_
  - _Uso:_ Apenas na ação de excluir (item "Excluir" do menu e botão do modal de confirmação).
- **Cores das Categorias:** Trabalho `#4285F4` (azul), Pessoal `#0B8043` (verde), Saúde `#D50000` (vermelho), Estudos `#8E24AA` (roxo) e Outro `#616161` (cinza)
  - _Uso:_ Borda esquerda do card do evento, badge com o nome da categoria e barra da categoria nas estatísticas.

### 3. Tipografia

Uma única família de fontes em todo o site, importada via Google Fonts:

- **Fonte:** `"Google Sans", Roboto, "Segoe UI", sans-serif`. Quem tem a Google Sans instalada vê ela; os outros veem a Roboto, carregada do Google Fonts (pesos 400, 500 e 700).
- **Títulos das páginas (h1):** de 22px no celular até 28px no desktop (peso 700), com tamanho fluido.
- **Títulos dos cards de evento:** 16px (peso 700).
- **Números das estatísticas:** 28px (peso 700).
- **Rótulos pequenos:** 12px em maiúsculas nos cards de resumo (peso 500) e 11px na legenda do menu do celular.

### 4. Diretrizes de Uso de Componentes

Regras para a aplicação dos componentes do Bootstrap na agenda:

- **Botões (`.btn`):** A ação principal da tela usa `.btn-primary` (azul). Cancelar usa `.btn-outline-secondary`, sem preenchimento. A exclusão usa `.btn-danger` e aparece só dentro do modal de confirmação.
- **Cards (`.card`):** Cada evento é um card com a borda esquerda e o badge na cor da categoria. A sombra é leve e aumenta quando o mouse passa por cima. Um evento concluído fica mais claro e com o título riscado.
- **Formulários (`.form-control`, `.form-select`):** O rótulo fica acima de cada campo. No celular, um campo por linha; a partir do tablet, os campos curtos (data, horários, categoria) ficam lado a lado.
- **Navegação:** No desktop, o menu fica no topo, com a página atual em azul e sublinhada. Abaixo de 992px, o menu vira uma barra com ícones na parte de baixo da tela.

### 5. Componentes do Framework no Protótipo

Componentes desenhados no protótipo que foram substituídos pelos componentes prontos do Bootstrap:

| No protótipo | Componente do Bootstrap | Onde aparece |
| --- | --- | --- |
| Barra superior com o nome do site e os links | Navbar (`.navbar`, `.navbar-nav`) | Todas as páginas |
| Card de cada evento, com a categoria | Card (`.card`) e Badge (`.badge`) | Início |
| Ícone de três pontos no card | Dropdown (`.dropdown`, `data-bs-toggle="dropdown"`) | Início |
| Confirmação antes de excluir (US-03) | Modal (`.modal`) | Início |
| Busca, filtro e campos do evento | Input group, Form control e Select | Início e Novo Evento |
| Barras de eventos por categoria | Progress (`.progress`) | Estatísticas |

### 6. Dos Tokens ao Código

Como cada token do Design System vira código. Todos ficam em `assets/scss/_variaveis.scss`: as variáveis Sass do Bootstrap são definidas **antes** do `@import` do framework; o que não tem variável no Bootstrap vira uma variável Sass própria, usada nos parciais. Os tamanhos são escritos em px, como no protótipo, e a função `px-para-rem()` os converte para rem.

| Token | Valor | No código |
| --- | --- | --- |
| Cor primária | `#4285F4` | `$cor-primaria` → `$primary` (Sass do Bootstrap) |
| Cor secundária (texto de apoio) | `#5F6368` | `$cor-texto-secundario` → `$secondary` e `$body-secondary-color` (Sass do Bootstrap) |
| Cor de perigo | `#D93025` | `$cor-perigo` → `$danger` (Sass do Bootstrap) |
| Cor do texto | `#202124` | `$cor-texto` → `$body-color` (Sass do Bootstrap) |
| Fundo dos componentes | `#FFFFFF` | `$cor-fundo` → `$body-bg` (Sass do Bootstrap) |
| Fundo da página | `#F8F9FA` | `$cor-fundo-secundario` (variável própria, usada no `body`) |
| Borda | `#DADCE0` | `$cor-borda` → `$border-color` (Sass do Bootstrap) |
| Hover | `#F1F3F4` | `$cor-hover` (variável própria, card "Adicionar novo evento") |
| Cores das categorias | 5 cores (seção 2) | `$cor-trabalho`, `$cor-pessoal`, `$cor-saude`, `$cor-estudos`, `$cor-outro` (variáveis próprias, aplicadas pelo mixin `cores-categoria`) |
| Fonte | `Google Sans`, `Roboto` | `$fonte-principal` → `$font-family-sans-serif` (Sass do Bootstrap) |
| Tamanhos de fonte | 11, 12, 16, 22 e 28px | `$fonte-xs`, `$fonte-sm`, `$fonte-lg`, `$fonte-xl`, `$fonte-xxl` (variáveis próprias) |
| Pesos da fonte | 500 e 700 | `$peso-medio`, `$peso-negrito` (variáveis próprias) |
| Espaçamentos | 2, 8, 16 e 24px | `$espaco-xs`, `$espaco-md`, `$espaco-lg`, `$espaco-xl` (variáveis próprias) |
| Raio da borda | 8px e 4px | `$raio` → `$border-radius`, `$raio-sm` → `$border-radius-sm` (Sass do Bootstrap) |
| Sombra leve | `0 1px 2px` | `$sombra-sm` → `$box-shadow-sm` (Sass do Bootstrap) e mixin `sombra-card` |
| Sombra do hover | `0 2px 6px` | `$sombra-md` (variável própria, no mixin `sombra-card`) |
