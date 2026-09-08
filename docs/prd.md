    # PRD — My Calendar

## 1. Identificação

| Campo       | Valor                            |
| ----------- | -------------------------------- |
| **Autor**   | Vinicius Liepienski de França    |
| **Projeto** | My Calendar — Agenda Pessoal Web |
| **Data**    | Agosto 2026                      |
| **Versão**  | 2.0                              |

---

## 2. Descrição

### Propósito

My Calendar é uma aplicação web de agenda pessoal que permite ao usuário registrar, visualizar, editar e excluir seus compromissos de forma simples e organizada. Os eventos são exibidos como cards em uma interface limpa, com busca, filtro por categoria e controle de conclusão.

### Problema Resolvido

Pessoas que precisam organizar compromissos pessoais (aulas, reuniões, lembretes, eventos recorrentes) frequentemente dependem de aplicativos pesados ou pouco personalizáveis. Falta uma solução leve, acessível via navegador, que permita gerenciar eventos como uma lista de cards simples, com categorização por cores, busca rápida e recorrência — tudo sem necessidade de instalação ou cadastro.

### Solução

Uma aplicação web que roda inteiramente no navegador, utiliza JSON Server como backend simulado e oferece ao usuário:

- Lista de eventos em cards, em layout responsivo (grade)
- Busca por texto (título e descrição) e filtro por categoria
- Marcador de "concluído" para controle de tarefas
- Recorrência (diário, semanal, mensal, anual)
- Formulário com validação (HTML nativo + regex) e busca de endereço via ViaCEP
- Indicação de feriados nacionais nos eventos (API Date Nager)
- Página de estatísticas com resumo da agenda
- Persistência via API REST + cache local

---

## 3. Atores do Sistema

| Ator        | Descrição                                                                                                                                                                   |
| ----------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Usuário** | Pessoa que acessa a aplicação para criar, visualizar, buscar, filtrar, editar e excluir eventos na agenda pessoal. É o único tipo de ator na versão 2.0 (sem autenticação). |

---

## 4. Histórias de Usuário (User Stories)

### US-01: Criar evento

**Como** Usuário,
**eu quero** criar um novo evento informando título, data, hora de início, hora de fim, descrição, categoria e recorrência,
**para que** eu possa registrar um compromisso na minha agenda.

**Critérios de Aceite:**

- Título é obrigatório (3–100 caracteres)
- Data e horas são obrigatórias
- Hora fim deve ser posterior à hora início
- Categoria deve ser selecionada entre as opções disponíveis
- Evento aparece na listagem imediatamente após criação

---

### US-02: Editar evento

**Como** Usuário,
**eu quero** editar um evento existente,
**para que** eu possa corrigir ou atualizar suas informações.

**Critérios de Aceite:**

- Ao clicar no card do evento, abre-se o formulário preenchido com os dados atuais
- Todos os campos podem ser alterados
- Alterações são salvas no JSON Server e no localStorage
- Card do evento é atualizado corretamente na listagem

---

### US-03: Excluir evento

**Como** Usuário,
**eu quero** excluir um evento que não será realizado,
**para que** eu possa manter minha agenda organizada e sem compromissos obsoletos.

**Critérios de Aceite:**

- Botão de exclusão disponível no formulário de edição
- Exibida mensagem de confirmação antes de excluir
- Evento é removido do JSON Server e do localStorage
- Card desaparece da listagem após confirmação

---

### US-04: Listar eventos em cards

**Como** Usuário,
**eu quero** ver meus eventos organizados como cards em uma grade responsiva,
**para que** eu consiga visualizar todos os meus compromissos de forma clara e rápida.

**Critérios de Aceite:**

- Cards exibidos em grade (adaptável a mobile e desktop)
- Cada card mostra: título, data, horário, categoria e descrição
- Eventos concluídos recebem destaque visual diferenciado
- Nova listagem ordenada por data (próximos eventos primeiro)

---

### US-05: Buscar eventos por texto

**Como** Usuário,
**eu quero** buscar eventos digitando um termo no campo de busca,
**para que** eu encontre rapidamente um compromisso pelo título ou descrição.

**Critérios de Aceite:**

- Campo de busca disponível no topo da página principal
- Busca filtra em tempo real por título e descrição (case-insensitive)
- Ao limpar o campo, todos os eventos são exibidos novamente

---

### US-06: Filtrar eventos por categoria

**Como** Usuário,
**eu quero** filtrar os eventos por categoria,
**para que** eu possa visualizar apenas os compromissos de interesse (ex: só trabalho ou só estudos).

**Critérios de Aceite:**

- Filtro por categoria disponível na página principal (select ou chips)
- Opções: Trabalho, Pessoal, Saúde, Estudos, Outro e "Todas"
- Ao selecionar uma categoria, apenas os eventos dela são exibidos
- Filtro combinado com a busca por texto

---

### US-07: Marcar evento como concluído

**Como** Usuário,
**eu quero** marcar um evento como concluído,
**para que** eu possa acompanhar o progresso das minhas tarefas.

**Critérios de Aceite:**

- Checkbox/controle de conclusão disponível no card do evento
- Evento concluído é persistido no JSON Server e no localStorage
- Card concluído recebe estilo visual diferenciado (ex: opacidade reduzida)

---

### US-08: Categorizar eventos

**Como** Usuário,
**eu quero** atribuir uma categoria (cor) aos meus eventos ao criá-los ou editá-los,
**para que** eu possa identificá-los visualmente na listagem.

**Critérios de Aceite:**

- Categorias disponíveis: Trabalho (azul), Pessoal (verde), Saúde (vermelho), Estudos (roxo), Outro (cinza)
- Categoria é selecionada via componente select no formulário
- Cards exibem a cor da categoria (badge ou destaque)
- Campo é obrigatório

---

### US-09: Criar eventos recorrentes

**Como** Usuário,
**eu quero** criar eventos que se repetem automaticamente (diário, semanal, mensal ou anual) com data fim,
**para que** eu não precise criar manualmente cada ocorrência de um compromisso recorrente.

**Critérios de Aceite:**

- Opções de recorrência: Nenhum, Diário, Semanal, Mensal, Anual
- Campo "data fim da recorrência" aparece apenas quando recorrência ≠ Nenhum
- Ao salvar, o sistema gera instâncias futuras do evento até a data fim
- Cada instância gerada pode ser editada ou excluída individualmente

---

### US-10: Buscar endereço via CEP

**Como** Usuário,
**eu quero** informar um CEP no formulário e obter o endereço automaticamente,
**para que** eu possa registrar o local do evento sem digitar todos os dados manualmente.

**Critérios de Aceite:**

- Campo CEP no formulário de evento (opcional)
- Ao sair do campo (blur), a aplicação consulta a API ViaCEP
- Logradouro, bairro, cidade e UF são preenchidos automaticamente
- Em caso de CEP inválido, exibida mensagem de erro amigável

---

### US-11: Visualizar estatísticas

**Como** Usuário,
**eu quero** ver estatísticas da minha agenda,
**para que** eu possa entender minha rotina e quantidade de compromissos.

**Critérios de Aceite:**

- Acesso à página de estatísticas pelo menu de navegação
- Exibe total de eventos cadastrados
- Exibe total de eventos por categoria
- Exibe quantidade de eventos concluídos e pendentes

### US-12: Visualizar feriados nacionais nos eventos

**Como** Usuário,
**eu quero** ver, no card do evento, um aviso quando a data coincidir com um feriado nacional,
**para que** eu possa planejar minha agenda sabendo quais dias não são úteis.

**Critérios de Aceite:**
- Ao carregar a listagem, a aplicação consulta os feriados nacionais do ano do período visualizado
- Quando a data de um evento coincide com um feriado, o card exibe um badge com o nome do feriado (ex: "Feriado — Carnaval")
- A listagem continua funcionando normalmente mesmo se a API de feriados estiver indisponível (o card é exibido sem o badge)

---

## 5. Regras de Negócio

| Código    | Regra                                                                                                                                         |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| **RN-01** | O título do evento é obrigatório, com mínimo de 3 caracteres e máximo de 100 caracteres.                                                      |
| **RN-02** | A hora de fim do evento deve ser sempre posterior à hora de início.                                                                           |
| **RN-03** | Quando um evento é configurado como recorrente, o campo "data fim da recorrência" torna-se obrigatório e deve ser posterior à data do evento. |
| **RN-04** | A exclusão de um evento recorrente oferece duas opções: excluir "apenas esta ocorrência" ou "esta e todas as ocorrências futuras".            |
| **RN-05** | Formatos obrigatórios: data no padrão AAAA-MM-DD; hora no padrão HH:MM (formato 24 horas); CEP no padrão 00000-000.                           |
| **RN-06** | As categorias disponíveis são pré-definidas e não editáveis: Trabalho, Pessoal, Saúde, Estudos, Outro.                                        |
| **RN-07** | Eventos recorrentes geram instâncias individualmente editáveis. A edição de uma instância não afeta as demais.                                |
| **RN-08** | O endereço do evento é opcional e, quando informado por CEP, é preenchido pela API ViaCEP (logradouro, bairro, cidade e UF).                  |
| **RN-09** | O campo "concluído" indica se o evento foi realizado. A alternância não altera os demais dados do evento.                                     |
| **RN-10** | A aplicação funciona sem autenticação. Todos os dados são armazenados localmente e no JSON Server simulado.                                   |
| **RN-11** | A indicação de feriado é informativa e não editável. Quando a API de feriados está indisponível, os eventos são exibidos sem a indicação, sem prejuízo às demais funcionalidades. |
