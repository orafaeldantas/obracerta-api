# ObraCerta — Instruções do Projeto

## Contexto

O ObraCerta é uma plataforma de gestão de obras residenciais. A primeira entrega é uma API REST em Java e Spring Boot, organizada como um monólito modular. O foco inicial é o controle financeiro da obra: orçamento, despesas e pagamentos.

Referências:

- Visão do produto, escopo do MVP e glossário de domínio: `docs/visao.md`
- Decisões arquiteturais: `docs/adr/`

## Stack

Java (LTS), Spring Boot, Spring Web, Spring Validation, Spring Data JPA, Spring Security, PostgreSQL, Flyway, JUnit 6, Mockito, Testcontainers, Maven, Docker e GitHub Actions.

Dependências novas só entram com justificativa e aprovação prévia.

## Comandos

```bash
./mvnw verify            # build completo com testes unitários e de integração
./mvnw test              # apenas testes unitários
./mvnw spring-boot:run   # executa a aplicação localmente
docker compose up -d     # sobe as dependências locais (PostgreSQL)
```

Testes de integração usam Testcontainers e exigem Docker em execução.

## Idioma

- **Código, identificadores, endpoints, mensagens de commit e logs:** inglês.
- **Documentação de produto e regras de negócio:** português (pt-BR).
- **README:** inglês, por ser a porta de entrada do repositório; aponta para a documentação em pt-BR.
- **Termos de domínio:** usar exclusivamente a tradução definida no glossário de `docs/visao.md`. Um termo novo entra no glossário antes de entrar no código.

## Arquitetura

### Organização

- Pacotes organizados **por módulo de negócio** (`construction`, `budget`, `planning`, `purchase`, `identity`), não por camada técnica.
- Dentro de cada módulo, separar API, aplicação, domínio e persistência à medida que a complexidade justificar.
- Um módulo não acessa repositórios nem entidades internas de outro módulo. A comunicação entre módulos passa pela interface pública (serviços de aplicação) do módulo de destino.
- Evitar dependências cíclicas entre módulos.

### Responsabilidades por camada

- **Controllers:** recebem a requisição, validam o formato da entrada, delegam ao serviço e mapeiam a resposta. Não contêm regras de negócio.
- **Serviços de aplicação:** orquestram casos de uso e definem as fronteiras transacionais.
- **Domínio:** concentra regras de negócio e invariantes. Entidades protegem o próprio estado; evitar modelos anêmicos com apenas getters e setters.
- **Persistência:** repositórios Spring Data restritos ao acesso a dados.

### Princípios de design

- Priorizar a solução mais simples que atenda ao requisito atual (KISS, YAGNI).
- Aplicar responsabilidade única e baixo acoplamento; preferir composição a herança.
- Usar injeção de dependência por construtor, com campos `final`.
- Criar interfaces apenas quando houver mais de uma implementação real ou uma fronteira clara a proteger.
- Nunca expor entidades JPA na API; usar DTOs (preferencialmente `record`) para entrada e saída.
- Preferir objetos imutáveis e value objects para conceitos do domínio (ex.: valores monetários).

## Convenções técnicas

### Dados e persistência

- Toda alteração de esquema é feita por migração Flyway. Migrações já aplicadas nunca são editadas; correções exigem uma nova migração.
- Integridade garantida também no banco: chaves estrangeiras, `NOT NULL`, `UNIQUE` e `CHECK` quando aplicável.
- `@Transactional` nos serviços de aplicação, nunca nos controllers. Consultas usam `readOnly = true`.
- Atenção a problemas de N+1: revisar o carregamento de associações e preferir `LAZY`.
- Registros financeiros não são apagados nem sobrescritos: cancelamentos e estornos geram novos registros, preservando o histórico.

### Valores monetários

- Sempre `BigDecimal`, nunca `double` ou `float`.
- Escala e `RoundingMode` explícitos e centralizados.
- Comparações com `compareTo`, nunca com `equals`.
- No banco, usar `NUMERIC` com precisão e escala definidas.

### API

- Recursos nomeados no plural e em inglês, sob o prefixo `/api/v1` (ex.: `/api/v1/constructions/{id}/expenses`). Ver ADR 0005.
- Métodos e códigos de status HTTP usados de acordo com sua semântica.
- Entradas validadas com Bean Validation nos DTOs.
- Erros padronizados no formato Problem Details (RFC 9457), com tratamento centralizado em `@RestControllerAdvice`.
- Contratos da API documentados (OpenAPI) e mantidos em sincronia com a implementação.

### Segurança

- Todo endpoint exige autenticação, salvo exceções explícitas e justificadas.
- Autorização verificada no acesso a cada recurso: um usuário nunca acessa dados de obras às quais não está vinculado.
- Credenciais, tokens e segredos nunca aparecem no código, nos logs ou nos commits; usar variáveis de ambiente.
- Não registrar dados pessoais ou financeiros sensíveis em logs.

## Testes

- Regras de domínio cobertas por testes unitários, sem dependência do Spring.
- Persistência e fluxos completos cobertos por testes de integração com Testcontainers e PostgreSQL real.
- Endpoints testados quanto a contrato, validação, autorização e respostas de erro.
- Testar cenários de falha e casos de borda, não apenas o fluxo principal.
- Toda correção de bug inclui um teste que reproduz o problema.
- Nomes de teste descrevem o comportamento esperado (ex.: `shouldRejectPaymentGreaterThanOutstandingAmount`).
- Nunca afirmar que um teste passou sem executá-lo, nem desativar ou enfraquecer testes para que passem.

## Fluxo de trabalho

1. Trabalhar em uma tarefa pequena e bem delimitada por vez.
2. Antes de implementar, identificar regras de negócio indefinidas ou ambíguas e perguntar, em vez de presumir.
3. Ao propor uma solução, explicar a decisão técnica e as alternativas relevantes consideradas.
4. Não alterar comportamento existente sem explicar o impacto.
5. Manter as alterações restritas ao escopo da tarefa; refatorações fora do escopo devem ser propostas separadamente.
6. Registrar decisões arquiteturais relevantes como ADR em `docs/adr/`.

### Critério de conclusão

Uma tarefa só está concluída quando:

- `./mvnw verify` passa localmente;
- novas regras de negócio possuem testes;
- as alterações foram revisadas (diff) e não há código morto, depuração esquecida ou TODOs sem contexto;
- a documentação afetada (glossário, OpenAPI, ADRs) foi atualizada.

## Git

- Commits pequenos, coesos e no padrão Conventional Commits, em inglês (ex.: `feat(budget): add expense registration`).
- Nunca executar `commit`, `push`, `rebase`, `reset --hard`, exclusão de branches ou qualquer comando destrutivo sem autorização explícita.
- Nunca fazer deploy ou publicar alterações sem autorização explícita.
- Não versionar arquivos de ambiente local, credenciais ou artefatos de build.
