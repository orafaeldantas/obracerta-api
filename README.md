# ObraCerta API

[![CI](https://github.com/orafaeldantas/obracerta-api/actions/workflows/ci.yml/badge.svg)](https://github.com/orafaeldantas/obracerta-api/actions/workflows/ci.yml)

REST API for managing residential construction and renovation projects, starting with financial control: budget, expenses and payments.

> **Status:** early development. The project foundation is in place, and the first business features are being specified.

## The problem

In small residential projects, information is spread across spreadsheets, notes, messaging apps and paper receipts. Simple questions become hard to answer: *How much have we spent? How much is committed but not yet paid? Which expenses were not planned?*

ObraCerta centralizes this information and preserves the history of every financial movement, so owners can make decisions based on reliable data.

## MVP scope

- Register and view a construction project
- Define the initial budget
- Record expenses linked to the project
- Record payments and outstanding amounts
- View a financial summary, including budget vs. actual

The full product vision, the MVP scope and the domain glossary are in [`docs/visao.md`](docs/visao.md) (Portuguese).

## Architecture

A **modular monolith** exposing a REST API: a single deployable unit and a single database, with code organized by business module (`construction`, `budget`, `planning`, `purchase`, `identity`) and explicit boundaries between modules.

Architectural decisions are recorded as ADRs in [`docs/adr/`](docs/adr/) (Portuguese).

| ADR | Decision |
|---|---|
| [0001](docs/adr/0001-registrar-decisoes-arquiteturais.md) | Record architectural decisions as ADRs |
| [0002](docs/adr/0002-monolito-modular.md) | Adopt a modular monolith |
| [0003](docs/adr/0003-pacotes-por-modulo-de-negocio.md) | Organize packages by business module |
| [0004](docs/adr/0004-java-25-e-spring-boot-4.md) | Use Java 25 and Spring Boot 4 |
| [0005](docs/adr/0005-versionamento-da-api-pela-url.md) | Version the API through the URL prefix |

## Tech stack

| | In use | Planned |
|---|---|---|
| Language | Java 25 (LTS) | |
| Framework | Spring Boot 4.1, Spring Web MVC, Bean Validation, Actuator | Spring Data JPA, Spring Security |
| Database | | PostgreSQL, Flyway |
| Tests | JUnit 6, Spring Boot Test | Mockito, Testcontainers |
| Build and CI | Maven (wrapper), GitHub Actions | Docker |

## Getting started

### Prerequisites

- [SDKMAN!](https://sdkman.io/) to install the JDK version pinned in [`.sdkmanrc`](.sdkmanrc)

Maven does not need to be installed: the project uses the Maven Wrapper (`./mvnw`).

### Run

```bash
sdk env install          # install and select the JDK pinned in .sdkmanrc
./mvnw verify            # build and run all tests
./mvnw spring-boot:run   # start the application on port 8080
```

The build fails if the JDK in use does not match the version declared in `pom.xml`.

### Health check

```bash
curl http://localhost:8080/actuator/health
```

## License

No license has been granted. All rights reserved.
