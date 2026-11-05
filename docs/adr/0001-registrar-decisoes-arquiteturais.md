# ADR 0001 — Registrar decisões arquiteturais com ADRs

- **Status:** Aceita
- **Data:** 2026-11-03

## Contexto

Decisões arquiteturais costumam ficar implícitas no código ou dispersas em conversas. Com o tempo, perde-se o motivo de cada escolha: quais alternativas foram consideradas, quais restrições existiam e quais custos foram aceitos. Sem esse registro, decisões são revertidas sem critério ou mantidas apenas por inércia, e novas pessoas no projeto não têm como entender por que o sistema é como é.

## Decisão

Registrar as decisões arquiteturais relevantes como ADRs (*Architecture Decision Records*) em `docs/adr/`, no formato proposto por Michael Nygard:

- Um arquivo Markdown por decisão, nomeado `NNNN-titulo-em-kebab-case.md`, com numeração sequencial;
- Seções: **Contexto**, **Decisão**, **Alternativas consideradas** e **Consequências**;
- Status: `Proposta`, `Aceita`, `Rejeitada`, `Obsoleta` ou `Substituída por ADR NNNN`;
- Escritos em português, como a documentação de produto;
- Um ADR aceito não é reescrito. Se a decisão mudar, um novo ADR a substitui e o antigo passa a apontar para ele;
- O índice em `docs/adr/README.md` é atualizado a cada novo ADR.

Uma decisão merece um ADR quando é difícil de reverter, afeta vários módulos ou envolve uma troca relevante entre alternativas.

## Alternativas consideradas

- **Documentar decisões no `CLAUDE.md` ou no README:** mistura regras vigentes com o histórico de como se chegou a elas, e não preserva o contexto de decisões substituídas.
- **Não registrar:** o custo inicial é zero, mas o motivo das decisões se perde.

## Consequências

- O histórico de decisões fica versionado junto com o código e revisável em cada mudança;
- Escrever um ADR tem um custo pequeno, aceito em troca da rastreabilidade;
- Decisões pequenas ou facilmente reversíveis não precisam de ADR, para que o processo não se torne burocrático.
