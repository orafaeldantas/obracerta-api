# Gestão de obras — Regras de negócio

| Item | Definição |
|---|---|
| **Módulo** | `construction` |
| **Status** | Aceita |
| **Versão** | 0.1 — 2026-11-16 |

Os termos seguem o glossário de [`visao.md`](../visao.md#8-conceitos-de-domínio).

---

## 1. Escopo

| Caso de uso | Situação |
|---|---|
| UC-01 — Cadastrar obra | Especificado neste documento |
| UC-02 — Consultar obra | Especificado neste documento |
| UC-03 — Listar obras | Especificado neste documento |
| Iniciar, concluir e cancelar obra | Estados definidos aqui; casos de uso especificados posteriormente |
| Alterar dados da obra | A especificar |
| Histórico de alterações da obra | A especificar |

---

## 2. Dados da obra

| Dado | Termo no código | Obrigatório | Regras |
|---|---|---|---|
| Nome | `name` | Sim | De 1 a 120 caracteres, desconsiderando espaços nas extremidades; não pode ser composto apenas de espaços |
| Endereço | `address` | Sim | Ver [seção 2.1](#21-endereço) |
| Início planejado | `plannedStartDate` | Sim | Data, sem horário; pode estar no passado |
| Término planejado | `plannedEndDate` | Sim | Data, sem horário; igual ou posterior ao início planejado |
| Estado | `status` | — | Definido pelo sistema; ver [seção 3](#3-estados) |
| Proprietário | `owner` | — | Definido pelo sistema: o usuário que cadastrou a obra |
| Data de cadastro | `createdAt` | — | Definida pelo sistema no momento do cadastro |

Dados definidos pelo sistema são ignorados se informados na entrada.

### 2.1. Endereço

| Dado | Termo no código | Obrigatório | Regras |
|---|---|---|---|
| CEP | `postalCode` | Sim | Exatamente 8 dígitos, sem hífen |
| Logradouro | `street` | Sim | Até 150 caracteres |
| Número | `number` | Sim | Até 10 caracteres; texto livre, para aceitar valores como `S/N` e `120A` |
| Complemento | `complement` | Não | Até 60 caracteres |
| Bairro | `district` | Sim | Até 80 caracteres |
| Cidade | `city` | Sim | Até 80 caracteres |
| UF | `state` | Sim | Sigla de uma das 27 unidades federativas |

Nos campos de texto, espaços nas extremidades são desconsiderados e um valor composto apenas de espaços equivale a um valor ausente.

A existência do CEP não é verificada: integrações externas estão fora do escopo do MVP.

---

## 3. Estados

| Estado | Termo no código | Descrição |
|---|---|---|
| Em planejamento | `PLANNED` | Obra cadastrada, ainda não iniciada |
| Em andamento | `IN_PROGRESS` | Execução iniciada |
| Concluída | `COMPLETED` | Execução encerrada com a entrega da obra |
| Cancelada | `CANCELLED` | Obra interrompida definitivamente, sem conclusão |

```
PLANNED ──► IN_PROGRESS ──► COMPLETED
   │             │
   └──────┬──────┘
          ▼
      CANCELLED
```

- Toda obra é cadastrada em `PLANNED`;
- Transições permitidas: `PLANNED → IN_PROGRESS`, `IN_PROGRESS → COMPLETED`, `PLANNED → CANCELLED` e `IN_PROGRESS → CANCELLED`;
- `COMPLETED` e `CANCELLED` são estados finais: nenhuma transição parte deles;
- Uma obra nunca é excluída. O cancelamento substitui a exclusão, preservando o histórico financeiro vinculado a ela.

---

## 4. Acesso

- Qualquer usuário autenticado pode cadastrar uma obra e se torna seu proprietário;
- Uma obra é acessível apenas ao seu proprietário;
- Uma obra de outro usuário é tratada como inexistente, sem revelar que ela existe.

> **Implementação provisória:** o módulo `identity` ainda não existe. Os casos de uso deste documento serão implementados primeiro sem autenticação, e o proprietário será incorporado assim que a autenticação estiver disponível, antes de qualquer funcionalidade financeira. Nenhuma versão sem controle de acesso deve ser implantada.

---

## 5. Casos de uso

### UC-01 — Cadastrar obra

1. O usuário informa nome, endereço, início planejado e término planejado;
2. O sistema valida os dados conforme a [seção 2](#2-dados-da-obra);
3. O sistema registra a obra no estado `PLANNED`, com o usuário como proprietário e a data de cadastro atual;
4. O sistema devolve a obra cadastrada, incluindo seu identificador.

Se algum dado for inválido, nada é registrado e o sistema indica cada dado inválido e o motivo.

### UC-02 — Consultar obra

1. O usuário informa o identificador de uma obra;
2. O sistema devolve os dados da obra.

Se a obra não existir, ou pertencer a outro usuário, o sistema informa que ela não foi encontrada.

### UC-03 — Listar obras

1. O sistema devolve as obras do usuário, das cadastradas mais recentemente para as mais antigas, em páginas.

---

## 6. Critérios de aceite

| # | Cenário | Resultado esperado |
|---|---|---|
| CA-01 | Cadastro com todos os dados válidos | Obra registrada em `PLANNED`, com identificador e data de cadastro |
| CA-02 | Cadastro sem complemento | Obra registrada normalmente |
| CA-03 | Nome ausente, vazio ou só com espaços | Cadastro rejeitado, indicando o nome |
| CA-04 | Nome com 121 caracteres | Cadastro rejeitado, indicando o nome |
| CA-05 | CEP com hífen, com letras ou com quantidade de dígitos diferente de 8 | Cadastro rejeitado, indicando o CEP |
| CA-06 | UF inexistente (ex.: `XX`) | Cadastro rejeitado, indicando a UF |
| CA-07 | Término planejado anterior ao início planejado | Cadastro rejeitado, indicando o término planejado |
| CA-08 | Término planejado igual ao início planejado | Obra registrada normalmente |
| CA-09 | Início planejado no passado | Obra registrada normalmente |
| CA-10 | Estado informado na entrada | Valor ignorado; obra registrada em `PLANNED` |
| CA-11 | Vários dados inválidos ao mesmo tempo | Cadastro rejeitado, indicando todos os dados inválidos |
| CA-12 | Consulta de obra inexistente | Obra não encontrada |
| CA-13 | Listagem sem obras cadastradas | Lista vazia |
| CA-14 | Listagem com várias obras | Obras ordenadas da mais recente para a mais antiga |

---

## 7. Questões em aberto

| Tema | Questão | Quando decidir |
|---|---|---|
| Financeiro | É possível registrar despesas ou pagamentos em uma obra concluída ou cancelada? | Especificação do módulo `budget` |
| Alteração | Quais dados podem ser alterados após o cadastro, e em quais estados? | Caso de uso de alteração |
| Adiamento | Uma obra adiada permanece em `PLANNED` e tem suas datas planejadas alteradas, sem um estado próprio. Falta definir se o adiamento é registrado no histórico | Caso de uso de alteração |
| Estados | Iniciar ou concluir a obra registra a data real de início ou término? | Casos de uso de transição |
