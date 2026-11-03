# ObraCerta — Visão do Produto

> Plataforma de gestão e acompanhamento de obras residenciais.

| Item | Definição |
|---|---|
| **Status** | Em concepção |
| **Categoria** | Gestão de obras (planejamento, execução e controle financeiro) |
| **Público inicial** | Construções e reformas residenciais |
| **Backend** | Java (LTS) + Spring Boot |
| **Arquitetura inicial** | Monólito modular expondo uma API REST |
| **Versão do documento** | 0.1 — 2026-10-20 |

---

## Sumário

1. [Visão geral](#1-visão-geral)
2. [Problema](#2-problema)
3. [Público-alvo](#3-público-alvo)
4. [Objetivos](#4-objetivos)
5. [Proposta de valor](#5-proposta-de-valor)
6. [Módulos do produto](#6-módulos-do-produto)
7. [Escopo do MVP](#7-escopo-do-mvp)
8. [Conceitos de domínio](#8-conceitos-de-domínio)
9. [Regras de negócio a definir](#9-regras-de-negócio-a-definir)
10. [Arquitetura técnica](#10-arquitetura-técnica)
11. [Requisitos de qualidade](#11-requisitos-de-qualidade)
12. [Processo de desenvolvimento](#12-processo-de-desenvolvimento)
13. [Critérios de sucesso](#13-critérios-de-sucesso)
14. [Riscos e questões em aberto](#14-riscos-e-questões-em-aberto)
15. [Roadmap](#15-roadmap)
16. [Histórico de revisões](#16-histórico-de-revisões)

---

## 1. Visão geral

O **ObraCerta** é uma plataforma digital para gestão e acompanhamento de obras, com foco inicial em construções e reformas residenciais.

Em obras desse porte, as informações costumam ficar dispersas entre planilhas, anotações, conversas de WhatsApp, recibos e documentos físicos. O ObraCerta centraliza esses dados em um único lugar, permitindo que proprietários e responsáveis acompanhem o andamento da obra, controlem os gastos, organizem etapas, registrem compras e tomem decisões com base em informações confiáveis.

A primeira entrega será uma API backend. Interfaces web e móveis serão consideradas em fases posteriores.

**Visão:** transformar informações dispersas sobre uma obra em dados organizados, histórico consultável e indicadores que apoiem a tomada de decisão.

---

## 2. Problema

Construir ou reformar um imóvel exige decisões financeiras, logísticas e operacionais contínuas. Mesmo em obras residenciais de pequeno porte, é preciso lidar com:

- Orçamento inicial e despesas imprevistas;
- Compra e entrega de materiais;
- Contratação e pagamento de profissionais;
- Planejamento e acompanhamento de etapas;
- Cumprimento de prazos;
- Alterações de escopo;
- Organização de comprovantes e documentos;
- Comunicação entre proprietário, prestadores e fornecedores.

Quando essas informações estão distribuídas entre diferentes ferramentas, obter uma visão confiável da situação da obra se torna difícil. Perguntas simples deixam de ter resposta imediata:

| Dimensão | Perguntas recorrentes |
|---|---|
| **Financeira** | Quanto já gastamos? Quanto ainda está disponível? Quanto foi comprometido, mas ainda não pago? |
| **Planejamento** | Quais etapas estão atrasadas? O que precisa ser feito a seguir? |
| **Compras** | Quais materiais precisamos comprar? Quanto pagamos por este material anteriormente? |
| **Controle de desvios** | Quais despesas não estavam previstas? Quais decisões provocaram aumento de custos? |

### Hipótese central

> Uma gestão mais organizada e transparente reduz a perda de informações, melhora o controle financeiro e facilita a tomada de decisões durante a obra.

Esta hipótese deve ser validada com potenciais usuários antes da expansão do produto.

---

## 3. Público-alvo

Os perfis abaixo são hipóteses iniciais. A priorização do MVP dependerá de qual perfil apresentar a dor mais relevante e maior disposição para adotar a solução.

| Perfil | Descrição | Necessidade principal |
|---|---|---|
| **Proprietário** | Pessoa que está construindo ou reformando um imóvel | Controle de gastos, prazos e andamento |
| **Responsável pela execução** | Quem coordena etapas, atividades e entregas | Organização do trabalho e atualização das informações |
| **Profissional da construção** | Pedreiros, eletricistas, encanadores, pintores e outros prestadores | Registro de atividades e participação no planejamento |
| **Fornecedor** *(futuro)* | Estabelecimentos que fornecem materiais | Cotações, propostas e acompanhamento de pedidos |

---

## 4. Objetivos

### 4.1. Objetivos de produto

- Centralizar as informações relevantes da obra;
- Facilitar o controle do orçamento e das despesas;
- Acompanhar etapas, atividades e prazos;
- Registrar compras, materiais e pagamentos;
- Preservar o histórico das movimentações relevantes;
- Evidenciar atrasos e desvios financeiros;
- Oferecer uma visão clara da situação atual da obra.

### 4.2. Objetivos de engenharia

- Disponibilizar uma API REST com contratos claros e documentados;
- Modelar o domínio com regras de negócio explícitas e testáveis;
- Garantir consistência dos dados por meio de persistência relacional e controle transacional;
- Proteger os dados com autenticação e autorização adequadas;
- Manter cobertura de testes unitários e de integração nas regras críticas;
- Automatizar verificações de qualidade com integração contínua;
- Registrar as decisões arquiteturais relevantes;
- Garantir execução reproduzível da aplicação em qualquer ambiente;
- Evoluir a arquitetura conforme necessidades reais, evitando complexidade prematura.

---

## 5. Proposta de valor

O diferencial do ObraCerta não está em telas de cadastro ou gráficos, mas em oferecer **uma visão confiável da obra**, preservando o histórico das decisões e relacionando planejamento, gastos, compras e execução.

Diferenciais a investigar e priorizar:

- **Rastreabilidade financeira** — histórico completo de cada movimentação;
- **Planejado × realizado** — comparação direta entre orçamento e execução;
- **Comprometido × pago** — visibilidade sobre obrigações assumidas e ainda não quitadas;
- **Integração entre compras e etapas** — materiais vinculados ao momento em que são necessários;
- **Histórico de preços** — referência para negociações e novas compras;
- **Alertas baseados em regras** — avisos de estouro de orçamento, atrasos e pendências;
- **Relatórios acessíveis** — informações compreensíveis para pessoas sem conhecimento técnico;
- **Colaboração** *(futuro)* — integração entre proprietários, profissionais e fornecedores.

---

## 6. Módulos do produto

Os módulos abaixo representam a visão de evolução do produto. Nem todos fazem parte da primeira versão.

| Módulo | Escopo | Fase |
|---|---|---|
| Gestão de obras | Cadastro, estado e histórico da obra | MVP |
| Controle financeiro | Orçamento, despesas e pagamentos | MVP |
| Planejamento e etapas | Etapas, atividades, prazos e progresso | Pós-MVP |
| Materiais e compras | Listas de compras, cotações, pedidos e entregas | Pós-MVP |
| Documentos e histórico | Comprovantes e trilha de auditoria | Pós-MVP |
| Indicadores e relatórios | Consolidação e visualização de dados | Pós-MVP |
| Prestadores de serviços | Contratação, propostas e avaliações | Futuro |

### 6.1. Gestão de obras

- Criar e consultar obras;
- Registrar endereço e informações gerais;
- Definir datas planejadas;
- Acompanhar o estado da obra;
- Registrar e consultar alterações relevantes.

### 6.2. Controle financeiro

- Definir o orçamento previsto;
- Registrar despesas e classificá-las por categoria;
- Controlar pagamentos e parcelas;
- Distinguir valores previstos, comprometidos e pagos;
- Consultar saldo disponível;
- Comparar orçamento inicial e realizado;
- Registrar estornos e cancelamentos preservando o histórico.

### 6.3. Planejamento e etapas

- Definir etapas, atividades e tarefas;
- Atribuir responsáveis e prazos;
- Registrar progresso e identificar atrasos;
- Definir dependências entre atividades.

### 6.4. Materiais e compras

- Cadastrar materiais e fornecedores;
- Criar listas de compras;
- Armazenar cotações;
- Registrar pedidos e entregas;
- Comparar preços históricos;
- Identificar materiais pendentes.

### 6.5. Documentos e histórico

- Organizar comprovantes;
- Associar documentos a despesas e compras;
- Registrar alterações de orçamento;
- Identificar autor e data de cada alteração relevante.

### 6.6. Indicadores e relatórios

- Orçamento previsto × realizado;
- Total pago e total pendente;
- Evolução dos gastos ao longo do tempo;
- Etapas concluídas e atrasadas;
- Compras por categoria;
- Variação de preços de materiais.

Todo indicador deve partir de dados confiáveis e de definições de negócio explícitas.

### 6.7. Prestadores de serviços

- Cadastrar prestadores e serviços oferecidos;
- Solicitar e comparar orçamentos;
- Registrar contratos e pagamentos;
- Avaliar serviços concluídos.

---

## 7. Escopo do MVP

O MVP deve ser pequeno o suficiente para ser entregue com qualidade e completo o suficiente para resolver um problema real: **o controle financeiro de uma obra**.

### 7.1. Incluído

| # | Funcionalidade |
|---|---|
| 1 | Cadastrar e consultar uma obra |
| 2 | Definir o orçamento inicial |
| 3 | Registrar despesas vinculadas à obra |
| 4 | Registrar pagamentos e valores pendentes |
| 5 | Consultar o resumo financeiro |
| 6 | Exibir a diferença entre orçamento e valores registrados |
| 7 | Garantir persistência e consistência dos dados |
| 8 | Disponibilizar a API documentada e testada |

O escopo definitivo depende da especificação das regras de negócio e da validação do problema.

### 7.2. Fora do escopo inicial

- Aplicativo móvel;
- Marketplace de prestadores;
- Integrações bancárias;
- Previsão de custos com inteligência artificial;
- Microsserviços ou arquitetura distribuída;
- Gestão de estoque;
- Integrações com sistemas de fornecedores;
- Módulos específicos de programas habitacionais.

Esses itens serão reavaliados mediante necessidade concreta.

---

## 8. Conceitos de domínio

Termos que exigem definição precisa para evitar cálculos ambíguos. As definições abaixo são preliminares e serão refinadas na especificação do MVP.

A documentação de produto é mantida em português; o código, em inglês. A coluna **Termo no código** estabelece a tradução oficial de cada conceito e deve ser usada de forma consistente em classes, atributos, endpoints e banco de dados. Novos termos devem ser incluídos nesta tabela antes de aparecerem no código.

| Termo | Termo no código | Definição preliminar |
|---|---|---|
| **Obra** | `Construction` | Empreendimento acompanhado pela plataforma, com orçamento, responsáveis e ciclo de vida próprios |
| **Orçamento** | `Budget` | Valor total previsto para a obra, podendo ser distribuído por categoria |
| **Categoria de despesa** | `ExpenseCategory` | Classificação usada para agrupar despesas e distribuir o orçamento |
| **Despesa** | `Expense` | Obrigação financeira vinculada à obra (material, serviço, taxa etc.) |
| **Pagamento** | `Payment` | Quitação total ou parcial de uma despesa |
| **Parcela** | `Installment` | Fração programada do pagamento de uma despesa |
| **Valor previsto** | `plannedAmount` | Montante planejado no orçamento, ainda sem obrigação assumida |
| **Valor comprometido** | `committedAmount` | Montante de despesas assumidas, independentemente de pagamento |
| **Valor pago** | `paidAmount` | Montante efetivamente quitado |
| **Valor pendente** | `outstandingAmount` | Diferença entre comprometido e pago |
| **Saldo disponível** | `availableBalance` | Diferença entre orçamento e valor comprometido |
| **Estorno** | `Reversal` | Reversão total ou parcial de um pagamento, registrada sem apagar o lançamento original |
| **Cancelamento** | `Cancellation` | Anulação de uma despesa, preservando seu registro no histórico |

---

## 9. Regras de negócio a definir

Cada módulo terá suas regras especificadas e documentadas antes da implementação.

### 9.1. Gestão de obras

- Quem pode criar uma obra?
- Uma obra pode ter vários responsáveis?
- Quais são os estados possíveis e suas transições?
- O que acontece quando uma obra é encerrada?
- Quais alterações devem ser registradas no histórico?

### 9.2. Financeiro

- O que caracteriza uma despesa?
- Como se distinguem despesa prevista, valor comprometido e pagamento realizado?
- Como funcionam pagamentos parciais e parcelados?
- Como são registrados estornos?
- O orçamento pode ser alterado após o início da obra? Com qual rastreabilidade?
- Como são tratadas despesas canceladas?

### 9.3. Planejamento

- Quais estados uma atividade pode assumir?
- Qual é o critério para considerar uma atividade atrasada?
- Uma etapa pode depender da conclusão de outra?
- Quem pode modificar prazos e progresso?

### 9.4. Acesso e segurança

- Quem pode consultar uma obra?
- Quem pode registrar despesas?
- Quem pode alterar o orçamento?
- Como garantir o isolamento entre dados de diferentes usuários?
- Como auditar alterações sensíveis?

---

## 10. Arquitetura técnica

### 10.1. Estilo arquitetural

A aplicação inicia como um **monólito modular** expondo uma **API REST**. Essa abordagem mantém a simplicidade operacional e preserva fronteiras claras entre os módulos de negócio, permitindo extração futura caso haja necessidade comprovada.

### 10.2. Stack

| Camada | Tecnologia |
|---|---|
| Linguagem | Java (LTS) |
| Framework | Spring Boot, Spring Web, Spring Validation |
| Persistência | Spring Data JPA / Hibernate |
| Banco de dados | PostgreSQL |
| Migrações | Flyway |
| Segurança | Spring Security |
| Testes | JUnit 5, Mockito, Testcontainers |
| Build | Maven |
| Containerização | Docker |
| CI | GitHub Actions |

As tecnologias serão incorporadas gradualmente, conforme a necessidade de cada entrega.

### 10.3. Organização em módulos

| Módulo | Responsabilidade |
|---|---|
| `construction` | Gestão de obras |
| `budget` | Orçamento, despesas e pagamentos |
| `planning` | Etapas e atividades |
| `purchase` | Compras e materiais |
| `identity` | Usuários, autenticação e autorização |

Cada módulo poderá conter camadas de API, aplicação, domínio e persistência na medida em que sua complexidade justificar. A estrutura física definitiva será estabelecida durante a implementação, evitando abstrações prematuras.

---

## 11. Requisitos de qualidade

- **Validação:** toda entrada externa é validada na fronteira da aplicação;
- **Isolamento do domínio:** regras de negócio independentes da camada HTTP;
- **Integridade:** restrições de integridade referencial garantidas no banco de dados;
- **Atomicidade:** operações compostas executadas em transações;
- **Precisão monetária:** valores representados com `BigDecimal` e regras de arredondamento explícitas;
- **Tratamento de erros:** respostas de erro padronizadas e consistentes;
- **Testabilidade:** regras de negócio e cenários de falha cobertos por testes;
- **Segurança:** recursos protegidos contra acesso não autorizado;
- **Auditoria:** alterações relevantes registradas com autor e data;
- **Versionamento do esquema:** toda mudança no banco realizada por migração;
- **Automação:** testes e verificações executados no pipeline de CI.

A complexidade de cada mecanismo deve ser proporcional ao risco e à necessidade da funcionalidade.

---

## 12. Processo de desenvolvimento

O desenvolvimento segue ciclos curtos e incrementais. Cada ciclo contempla:

1. **Objetivo** — meta pequena e explícita;
2. **Execução** — implementação ou investigação concreta;
3. **Verificação** — validação do resultado por testes ou inspeção;
4. **Registro** — documentação das decisões e descobertas;
5. **Próximo passo** — definição clara da continuidade.

Decisões de negócio e arquiteturais são registradas e revisadas antes de serem incorporadas ao código.

---

## 13. Critérios de sucesso

### 13.1. Produto

- O sistema resolve um fluxo de trabalho bem identificado;
- Os usuários compreendem as informações apresentadas;
- O controle financeiro apresenta valores coerentes e auditáveis;
- A solução reduz a dispersão de informações;
- As funcionalidades são úteis para o público priorizado.

### 13.2. Engenharia

- A aplicação é executável a partir de instruções documentadas;
- As principais regras de negócio possuem testes automatizados;
- A API possui contratos claros e documentados;
- Os dados mantêm integridade em cenários de falha;
- O acesso aos recursos é devidamente autorizado;
- O pipeline de CI valida cada mudança;
- As decisões arquiteturais estão registradas e justificadas.

### 13.3. Entrega

- Repositório organizado e documentado;
- Histórico de mudanças consistente e rastreável;
- Ambiente de demonstração funcional;
- Testes reproduzíveis em qualquer ambiente.

---

## 14. Riscos e questões em aberto

| Tema | Questão |
|---|---|
| Público | Qual perfil de usuário apresenta a dor mais urgente? |
| Problema | O maior problema é financeiro, operacional ou de comunicação? |
| Comportamento atual | Como o público controla suas obras hoje? |
| Informação | Quais informações são mais difíceis de obter? |
| Adoção | O usuário está disposto a registrar os dados necessários? |
| Retenção | Quais funcionalidades justificam o uso contínuo? |
| Escopo | Qual é o escopo mínimo que entrega valor real? |
| Diferenciação | Como superar planilhas e aplicativos existentes? |
| Validação | Como validar a proposta antes de investir em módulos extensos? |

Essas questões orientam as próximas etapas de investigação.

---

## 15. Roadmap

| Fase | Entregas |
|---|---|
| **Descoberta** | Refinar o público e o problema prioritário; entrevistar potenciais usuários; levantar situações reais |
| **Especificação** | Definir o fluxo principal do MVP; especificar suas regras de negócio; elaborar o modelo de domínio inicial |
| **Construção** | Estruturar a aplicação Spring Boot; implementar e testar a primeira funcionalidade; evoluir a API em entregas pequenas |
| **Validação** | Publicar uma demonstração; coletar feedback; revisar o escopo com base nas evidências |

---

## 16. Histórico de revisões

| Versão | Data | Alteração | Motivo |
|---|---|---|---|
| 0.1 | 2026-10-20 | Criação da visão inicial | Organizar proposta, problema, escopo e hipóteses |
| 0.2 | 2026-10-22 | Reestruturação do documento | Padronizar formato, consolidar conceitos de domínio e roadmap |
| 0.3 | 2026-10-22 | Inclusão dos termos no código no glossário de domínio | Garantir tradução consistente entre a documentação (pt-BR) e o código (inglês) |

---

> **Princípio orientador:** construir primeiro uma solução pequena para um problema real, validar seu valor e evoluir com base em evidências, mantendo qualidade técnica e melhoria contínua.
