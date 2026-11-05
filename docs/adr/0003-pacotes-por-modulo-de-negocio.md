# ADR 0003 — Organizar pacotes por módulo de negócio

- **Status:** Aceita
- **Data:** 2026-11-03

## Contexto

O [ADR 0002](0002-monolito-modular.md) define módulos de negócio com fronteiras explícitas. A organização dos pacotes Java precisa refletir essas fronteiras: quem lê a estrutura do projeto deve enxergar o domínio, não apenas a tecnologia.

Em Java, o pacote também é uma ferramenta de encapsulamento: classes sem modificador de acesso (*package-private*) só são visíveis dentro do próprio pacote.

## Decisão

Organizar o código **por módulo de negócio** sob `br.com.obracerta`:

```
br.com.obracerta
├── construction
├── budget
├── planning
├── purchase
└── identity
```

- Dentro de cada módulo, subpacotes (`api`, `application`, `domain`, `persistence`) são criados **apenas quando a complexidade do módulo justificar**. Um módulo pequeno pode começar com todas as classes em um único pacote;
- Classes que não fazem parte da interface pública do módulo devem ser *package-private* sempre que possível;
- Código transversal (tratamento de erros, configuração de segurança) fica em pacotes próprios, criados quando surgirem, sem acumular regras de negócio.

## Alternativas consideradas

- **Pacotes por camada técnica** (`controller`, `service`, `repository`): é o formato mais comum em tutoriais, mas espalha cada funcionalidade por vários pacotes, obriga as classes a serem `public` para se enxergarem entre camadas e não expressa as fronteiras entre módulos.
- **Arquitetura hexagonal completa desde o início** (portas e adaptadores em todos os módulos): protege bem o domínio, mas adiciona interfaces e camadas antes que exista complexidade que as justifique.

## Consequências

- A estrutura de pastas comunica o domínio e as fronteiras do [ADR 0002](0002-monolito-modular.md);
- Restringir a visibilidade com *package-private* depende de disciplina, pois o padrão das IDEs e dos geradores é criar classes `public`;
- O `HealthController` existente em `br.com.obracerta.api.health` não segue esta organização. Sua permanência será decidida junto com a adoção do Spring Boot Actuator.
