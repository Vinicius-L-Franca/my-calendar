# PRD — My Calendar

## 1. Identificação

| Campo | Valor |
|-------|-------|
| **Autor** | Vinicius Liepienski de França |
| **Projeto** | My Calendar — Agenda Pessoal Web |
| **Data** | Agosto 2026 |
| **Versão** | 1.0 |

---

## 2. Descrição

### Propósito

My Calendar é uma aplicação web de calendário/agenda projetada para organização pessoal. O sistema permite ao usuário criar, visualizar, editar e excluir compromissos cominterface limpa inspirada no Google Calendar.

### Problema Resolvido

Pessoas que precisam organizar compromissos pessoais (aulas, reuniões, lembretes, eventos recorrentes) frequentemente dependem de aplicativos pesados ou pouco personalizáveis. Falta uma solução leve, acessível via navegador, que permita gerenciar eventos com visualização flexível (mês/semana/dia), categorização por cores e recorrência simples — tudo sem necessidade de instalação.

### Solução

Uma aplicação web que roda inteiramente no navegador, utiliza JSON Server como backend simulado, e oferece ao usuário:

- Três visões de calendário (mensal, semanal, diária)
- Categorização de eventos com cores
- Recorrência simples (diário, semanal, mensal, anual)
- Drag-and-drop para reagendamento rápido
- Persistência via API REST + cache local

---

## 3. Atores do Sistema

| Ator | Descrição |
|------|-----------|
| **Usuário** | Pessoa que acessa a aplicação para criar, visualizar, editar e excluir eventos na agenda pessoal. É o único tipo de ator na versão 1.0 (sem autenticação). |

---

## 4. Histórias de Usuário (User Stories)

### US-01: Criar evento

**Como** Usuário,
**eu quero** criar um novo evento informando título, data, hora de início, hora de fim, descrição e categoria,
**para que** eu possa registrar um compromisso na minha agenda.

**Critérios de Aceite:**
- Título é obrigatório (3–100 caracteres)
- Data e horas são obrigatórias
- Hora fim deve ser posterior à hora início
- Categoria deve ser selecionada entre as opções disponíveis
- Evento aparece imediatamente no calendário após criação

---

### US-02: Editar evento

**Como** Usuário,
**eu quero** editar um evento existente clicando nele,
**para que** eu possa corrigir ou atualizar suas informações.

**Critérios de Aceite:**
- Ao clicar no evento, abre-se o formulário preenchido com os dados atuais
- Todos os campos podem ser alterados
- Alterações são salvas no JSON Server e no localStorage
- Evento atualizado é exibido corretamente no calendário

---

### US-03: Excluir evento

**Como** Usuário,
**eu quero** excluir um evento que não será realizado,
**para que** eu possa manter minha agenda organizada e sem compromissos obsoletos.

**Critérios de Aceite:**
- Botão de exclusão disponível no formulário de edição
- Exibida mensagem de confirmação antes de excluir
- Evento é removido do JSON Server e do localStorage
- Evento desaparece do calendário após confirmação

---

### US-04: Visualizar eventos em mensal

**Como** Usuário,
**eu quero** ver meus eventos em uma visão mensal (grid de dias),
**para que** eu possa planejar meu mês de forma ampla.

**Critérios de Aceite:**
- Exibido grid com todos os dias do mês atual
- Eventos aparecem como fichas coloridas (conforme categoria) dentro dos dias
- Dia atual destacado visualmente
- Navegação entre meses disponível

---

### US-05: Visualizar eventos em semanal

**Como** Usuário,
**eu quero** ver meus eventos em uma visão semanal (7 colunas com faixas horárias),
**para que** eu possa organizar minha rotina de cada semana com mais detalhe.

**Critérios de Aceite:**
- 7 colunas representando dom–sab
- Faixas horárias das 6h às 22h
- Eventos posicionados conforme hora de início e duração
- Dia atual destacado
- Navegação entre semanas disponível

---

### US-06: Visualizar eventos em diária

**Como** Usuário,
**eu quero** ver meus eventos em uma visão diária (coluna única com faixas horárias),
**para que** eu possa ver os detalhes do dia atual com precisão.

**Critérios de Aceite:**
- Coluna única com faixas horárias das 6h às 22h
- Eventos posicionados conforme hora de início e duração
- Data exibida no topo
- Navegação entre dias disponível

---

### US-07: Navegar entre períodos

**Como** Usuário,
**eu quero** navegar entre meses, semanas e dias usando botões de seta,
**para que** eu possa visualizar compromissos passados e futuros.

**Critérios de Aceite:**
- Botões de seta (anterior/próximo) em todas as visões
- Na visão mensal: navega mês a mês
- Na visão semanal: navega semana a semana
- Na visão diária: navega dia a dia
- Período atual exibido no topo (ex: "Agosto 2026", "18–24 Ago 2026", "25 Ago 2026")

---

### US-08: Voltar à data atual

**Como** Usuário,
**eu quero** um botão "Hoje" para retornar rapidamente à data atual,
**para que** eu não precise navegar manualmente até o dia de hoje.

**Critérios de Aceite:**
- Botão "Hoje" visível em todas as visões
- Ao clicar, a visualização centraliza na data atual
- Dia atual é destacado após a navegação

---

### US-09: Categorizar eventos

**Como** Usuário,
**eu quero** atribuir uma categoria (cor) aos meus eventos ao criá-los ou editá-los,
**para que** eu possa identificá-los visualmente no calendário.

**Critérios de Aceite:**
- Categorias disponíveis: Trabalho (azul), Pessoal (verde), Saúde (vermelho), Estudos (roxo), Outro (cinza)
- Categoria é selecionada via componente select no formulário
- Eventos exibidos com a cor da categoria no calendário
- Campo é obrigatório

---

### US-10: Criar eventos recorrentes

**Como** Usuário,
**eu quero** criar eventos que se repetem automaticamente (diário, semanal, mensal ou anual) com data fim,
**para que** eu não precise criar manualmente cada ocorrência de um compromisso recorrente.

**Critérios de Aceite:**
- Opções de recorrência: Nenhum, Diário, Semanal, Mensal, Anual
- Campo "data fim da recorrência" aparece apenas quando recorrência ≠ Nenhum
- Ao salvar, o sistema gera instâncias futuras do evento até a data fim
- Cada instância gerada pode ser editada ou excluída individualmente

---

### US-11: Arrastar eventos (Drag-and-drop)

**Como** Usuário,
**eu quero** arrastar um evento para reagendá-lo em outro dia ou horário nas visões semanal e diária,
**para que** eu possa reorganizar minha agenda de forma rápida e intuitiva.

**Critérios de Aceite:**
- Eventos são arrastáveis (draggable) nas visões semanal e diária
- Ao soltar o evento, a data e/ou hora são atualizadas automaticamente
- Alteração é persistida no JSON Server e no localStorage
- Feedback visual durante o arraste (sombra, opacidade)

---

## 5. Regras de Negócio

| Código | Regra |
|--------|-------|
| **RN-01** | O título do evento é obrigatório, com mínimo de 3 caracteres e máximo de 100 caracteres. |
| **RN-02** | A hora de fim do evento deve ser sempre posterior à hora de início. |
| **RN-03** | Quando um evento é configurado como recorrente, o campo "data fim da recorrência" torna-se obrigatório e deve ser posterior à data do evento. |
| **RN-04** | A exclusão de um evento recorrente oferece duas opções: excluir "apenas esta ocorrência" ou "esta e todas as ocorrências futuras". |
| **RN-05** | Formatos obrigatórios: data no padrão AAAA-MM-DD; hora no padrão HH:MM (formato 24 horas). |
| **RN-06** | As categorias disponíveis são pré-definidas e não editáveis: Trabalho, Pessoal, Saúde, Estudos, Outro. |
| **RN-07** | Eventos recorrentes geram instâncias individualmente editáveis. A edição de uma instância não afeta as demais. |
| **RN-08** | A aplicação funciona sem autenticação. Todos os dados são armazenados localmente e no JSON Server simulado. |
