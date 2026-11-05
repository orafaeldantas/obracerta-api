# ADR 0002 — Adotar um monólito modular

- **Status:** Aceita
- **Data:** 2026-11-03

## Contexto

O ObraCerta está em fase de concepção. O MVP cobre o controle financeiro de uma obra, e o produto prevê outros módulos (planejamento, compras, documentos). As fronteiras entre esses módulos ainda não foram validadas com uso real.

O projeto é desenvolvido por uma única pessoa, e a operação precisa ser simples: um artefato, um banco de dados e um pipeline.

## Decisão

Construir a aplicação como um **monólito modular**: um único deployable Spring Boot e um único banco PostgreSQL, com o código dividido em módulos de negócio (`construction`, `budget`, `planning`, `purchase`, `identity`) e fronteiras explícitas.

Regras de fronteira:

- Um módulo não acessa repositórios nem entidades internas de outro módulo;
- A comunicação entre módulos passa pelos serviços de aplicação públicos do módulo de destino;
- Dependências cíclicas entre módulos não são permitidas.

## Alternativas consideradas

- **Microsserviços:** permitem implantar e escalar módulos de forma independente, mas trazem custos de rede, consistência distribuída, observabilidade e operação que não se justificam com um desenvolvedor e um domínio ainda não validado. Fronteiras erradas entre serviços são caras de corrigir.
- **Monólito em camadas, sem módulos:** é o mais simples no início, mas tende a gerar acoplamento entre áreas de negócio, o que dificulta evoluir ou extrair partes no futuro.

## Consequências

- Uma única transação de banco pode cobrir uma operação inteira, o que simplifica a consistência;
- Corrigir uma fronteira mal definida custa uma refatoração interna, não uma migração entre serviços;
- Se houver necessidade comprovada, um módulo com fronteiras bem mantidas pode ser extraído como serviço;
- Por enquanto, as fronteiras são garantidas apenas por convenção e revisão. Quando houver mais de um módulo implementado, avaliar uma verificação automática (Spring Modulith ou ArchUnit), o que exigirá uma nova dependência e aprovação prévia.
