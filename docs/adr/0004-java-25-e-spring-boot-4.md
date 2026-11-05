# ADR 0004 — Usar Java 25 e Spring Boot 4

- **Status:** Aceita
- **Data:** 2026-11-03

## Contexto

O projeto começa do zero, sem código legado nem restrição de plataforma. A stack definida é Java (LTS) com Spring Boot. Duas versões LTS do Java estão disponíveis: a 21 (setembro de 2023) e a 25 (setembro de 2025).

O build também precisa ser reproduzível: a mesma versão de JDK e de Maven em qualquer máquina e no CI.

## Decisão

- **Java 25 (LTS)**, com a distribuição Eclipse Temurin;
- **Spring Boot 4.1**, usando `spring-boot-starter-parent` como POM pai;
- A versão do JDK é declarada uma única vez, na propriedade `java.version` do `pom.xml`, e reproduzida em três pontos:
  - `.sdkmanrc`: seleciona o JDK localmente com o SDKMAN (`sdk env`);
  - `maven-enforcer-plugin`: interrompe o build na fase `validate` se o JDK em uso não for da versão declarada;
  - pipeline de CI: configura a mesma versão do JDK;
- O Maven é executado sempre pelo Maven Wrapper (`./mvnw`), que fixa a versão do Maven no repositório.

## Alternativas consideradas

- **Java 21:** tem mais tempo de uso em produção e mais exemplos da comunidade, mas termina seu suporte antes da 25. Migrar depois teria um custo pequeno, mas desnecessário em um projeto novo.
- **Spring Boot 3.x:** é a versão da maioria dos tutoriais atuais, mas já está em fim de ciclo e exigiria migração para a 4 em pouco tempo.
- **Selecionar o JDK por um script com caminho fixo:** funciona apenas na máquina em que foi escrito.
- **Maven Toolchains:** garante o JDK de forma mais robusta, mas exige configuração em `~/.m2` em cada máquina, o que é complexidade além do necessário agora.

## Consequências

- Recursos recentes da linguagem (*records*, *pattern matching*, *sealed classes*, *virtual threads*) ficam disponíveis;
- Parte da documentação e dos exemplos da comunidade ainda usa Spring Boot 3 e Java 17/21. Exemplos devem ser conferidos na documentação oficial da versão usada;
- Bibliotecas novas precisam ser compatíveis com Java 25 e Spring Boot 4 antes de serem adotadas;
- Trocar a versão do JDK exige alterar `java.version`, o `.sdkmanrc` e a configuração do CI na mesma mudança.
