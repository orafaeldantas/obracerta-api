# ADR 0005 — Versionar a API pelo prefixo da URL

- **Status:** Aceita
- **Data:** 2026-11-03

## Contexto

A API REST é o primeiro produto do ObraCerta e será consumida por clientes futuros (web e móvel). Mudanças incompatíveis no contrato precisam acontecer sem quebrar clientes existentes, e a estratégia de versionamento deve ser definida antes do primeiro endpoint de negócio.

## Decisão

- Todos os endpoints da API ficam sob o prefixo **`/api/v1`** (ex.: `/api/v1/constructions/{id}/expenses`);
- O prefixo é declarado explicitamente no `@RequestMapping` de cada controller;
- A versão só muda (`v2`) em uma **mudança incompatível**: remover ou renomear um campo, mudar seu tipo ou seu significado, ou alterar o comportamento de um endpoint existente;
- Mudanças **compatíveis** (novos endpoints, novos campos opcionais na entrada, novos campos na resposta) não mudam a versão. Clientes devem ignorar campos desconhecidos;
- Endpoints de infraestrutura (ex.: health checks do Actuator) não são versionados.

## Alternativas consideradas

- **Versão em header ou media type** (`Accept: application/vnd.obracerta.v1+json`): mantém as URLs estáveis, mas é menos visível, mais difícil de testar com ferramentas simples e mais complexo de documentar.
- **Sem versionamento:** é o mais simples, mas obrigaria a quebrar clientes ou a criar uma estratégia às pressas na primeira mudança incompatível.
- **Suporte nativo a versionamento do Spring Framework 7:** resolve a versão por header, parâmetro ou segmento de caminho e roteia para o handler correspondente. Será reavaliado se houver necessidade real de manter duas versões em paralelo.

## Consequências

- A versão fica visível em logs, na documentação OpenAPI e em testes manuais com `curl`;
- Declarar o prefixo em cada controller é repetitivo, mas é explícito e fácil de verificar nos testes;
- Uma futura `v2` provavelmente exigirá manter controllers ou DTOs paralelos durante a transição.
